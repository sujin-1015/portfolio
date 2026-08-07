---
title: "소상공인 대출 사전심사 & 자동 소액대출 집행 AI Agent 구현"
period: "2026/07/20 → 2026/08/03"
order: 3
cover: "/assets/images/projects/10-ai-agent-loan/cover.jpg"
badge: "2026 Google Cloud & Solana AI Agentic Hackathon 결승 진출 (총 60개 팀 중 10개 팀, 2026.08.21 최종 발표 예정)"
field: ["금융", "핀테크", "소상공인대출", "블록체인", "AI"]
skills: ["Python", "AI Agent", "LLM", "RAG", "XGBoost", "FastAPI", "GCP", "Solana"]
---

매출 데이터와 사업자 설명을 함께 분석해 대출 여부를 판단하고, 승인된 대출을 Solana devnet을 통해 즉시 자동 집행하는 AI Agent 시스템 **CreditFlow AI Agent**를 구현했습니다.

## 1. 문제 정의

소상공인은 매출 변동성이 크고 사업 상황이 빠르게 변화하기 때문에, 신용점수와 정형 데이터만으로 실제 상환 능력과 사업 지속 가능성을 충분히 평가하기 어렵습니다.

**기존 대출 심사의 문제**

- **정형 심사의 한계** — 신용점수·담보·재무정보 중심의 심사에서는 매출 회복이나 일시적 부진의 원인과 같은 사업 맥락을 반영하기 어렵습니다.
- **정량·정성 정보의 단절** — 매출 및 재무 데이터와 사업자의 실제 사업 상황이 서로 분리된 상태로 평가됩니다.
- **심사와 자금 집행의 단절** — 대출 승인 이후에도 계약 및 계좌 확인 등의 별도 절차가 필요해 실제 자금 집행까지 시간이 소요됩니다.
- **높은 심사 운영 비용** — 대출 금액이 작더라도 담당자가 서류를 직접 검토해야 하므로 건별 심사 비용이 크게 발생합니다.

**Hypothesis** — 정량 데이터만으로는 사업자의 지속 가능성을 충분히 설명하기 어렵습니다. 따라서 정량적 위험도와 정성적 사업 맥락을 자동으로 함께 분석하고, 심사 결과를 실제 자금 집행까지 연결하는 AI Agent가 필요할 것이라 가정했습니다.

## 2. 서비스 구현

### 2-1. 정량 심사 — XGBoost 기반 부도확률 예측

- Kaggle 개인 대출 데이터를 소상공인 맥락으로 재해석해 활용 (컬럼 재정의, 예: Income → 매출, 근속연수 → 업력)
- 대출 신청자의 부도확률을 예측하는 머신러닝 모델 구축 — 총 18개 피처 활용, XGBoost 및 LightGBM 모델 학습
- 부도확률 임계값 58.8% / 62.6%를 기준으로 3단계 위험 등급 산정

| 등급 | 기준 |
|---|---|
| 승인 | 부도확률 ≤ 58.8% |
| 조건부승인 | 58.8% < 부도확률 ≤ 62.6% |
| 거절 | 부도확률 > 62.6% |

**Validation AUC** — XGBoost: 0.7846 / LightGBM: 0.7794. 검증 성능을 기준으로 XGBoost를 최종 서빙 모델로 채택했습니다.

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-01.png' | relative_url }}" alt="XGBoost와 LightGBM의 Validation AUC 비교 바 차트">
  <figcaption>모델별 Validation AUC 비교</figcaption>
</figure>

등급별 실제 부도율은 위험 등급이 높아질수록 증가해, 1차 정량 심사가 대출 위험도를 단계적으로 구분하고 있음을 확인했습니다.

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-02.png' | relative_url }}" alt="승인/조건부승인/거절 등급별 실제 부도율 — 7.66%, 28.09%, 43.66%">
  <figcaption>등급별 실제 부도율</figcaption>
</figure>

### 2-2. 정성 심사 — LLM + RAG 기반 최종 판단

**정성 판단 과정**: 사업자 설명 → Gemini 요약 → RAG 정책 검색 → 정량 등급과 결합 → 최종 판정

- Gemini를 활용해 사업자 설명에서 매출 추세, 긍정 신호, 위험 신호를 구조화해 추출
- 자체 구현한 코사인 유사도 기반 RAG를 통해 정책 문서에서 해당 신청 건과 관련된 심사 기준 검색
- XGBoost 정량 위험 등급, 사업자의 정성적 사업 상황, 정책 문서의 심사 근거 3가지를 종합해 최종 대출 여부 판단

**LLM 판단 범위 제한 (프롬프트 기반)** — LLM이 머신러닝의 위험 판단을 임의로 뒤집지 못하도록, 프롬프트에 판정 가능 범위를 명시적으로 규정했습니다.

- 승인 ↔ 조건부승인 가능
- 거절 → 조건부승인/승인 불가

즉, 정성적 맥락은 경계 구간의 판단을 보완하는 역할로 한정하고, 높은 위험도로 분류된 신청자를 LLM이 임의로 승인하지 못하도록 유도했습니다. 또한 Gemini의 Function Calling을 강제해 최종 판정, 판단 근거, 산정 금액 등을 구조화된 형태로 반환하도록 구현했습니다.

### 2-3. 온체인 자동 집행 — Solana devnet

최종 심사가 완료되면 판정 결과에 따라 Solana devnet에서 실제 자금 집행 로직까지 자동 실행되도록 구현했습니다.

| 최종 판정 결과 | 실행 내용 |
|---|---|
| 승인 | Circle 공식 devnet USDC 1.00 USDC를 신청자 지갑으로 즉시 송금 |
| 조건부승인 | 0.50 USDC를 우선 집행 후 자동 재심사를 통해 조건이 충족되면 나머지 0.50 USDC 추가 집행 |
| 거절 | 자금 미집행. 판정 결과 및 판단 근거만 시스템에 기록 |

**온체인 판단 근거 검증** — 대출 판정 근거를 SHA-256 Hash로 변환하고 이를 SPL Memo에 트랜잭션과 함께 기록해, 대출 실행 이후 판단 근거가 변경되지 않았는지 무결성을 검증할 수 있도록 했습니다. 최종적으로 최종 판정, 산정 대출 금액, 판정 근거 Hash, Solana tx_signature 정보를 BigQuery에 저장해 AI의 판단 → 자금 집행 → 사후 감사까지 하나의 기록 흐름으로 연결했습니다.

### 2-4. 이벤트 기반 인프라 및 자동 재심사

대출 심사뿐만 아니라 집행 이후 기록과 조건부승인 재심사까지 자동화했습니다.

- **동기 심사 및 집행**: FastAPI → Cloud Run → Gemini/XGBoost → Solana → BigQuery. 사용자가 심사를 요청하면 Cloud Run에서 정량·정성 심사와 Solana 송금을 하나의 흐름으로 처리
- **이벤트 기반 결제 영수증 기록**: Pub/Sub → Eventarc → Cloud Workflows → BigQuery. 대출 집행 이벤트를 비동기로 전달해 별도의 영수증 및 실행 이력을 기록
- **조건부승인 자동 재심사**: Cloud Scheduler → FastAPI → 재심사 → BigQuery. Cloud Scheduler가 매일 03:00 KST에 조건부승인 대출을 확인해 재심사 대상을 자동 조회 (실제 재심사 처리는 합성 후속 텍스트를 준비한 신청자에 한해 검증 완료)
- **통합 모니터링**: 동기 심사, 이벤트 기록, 자동 재심사 결과를 동일한 BigQuery 데이터셋에 저장해 하나의 대시보드에서 대출의 전체 Lifecycle을 조회할 수 있도록 구현

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-05.png' | relative_url }}" alt="동기 심사·집행, 이벤트 기반 영수증 생성, 스케줄 재심사 3단계 인프라 구조도">
  <figcaption>이벤트 기반 인프라 구조</figcaption>
</figure>

심사 대시보드에서 온체인 증빙을 확인할 수 있습니다 ([creditflow-agent-46585987317.asia-northeast3.run.app](https://creditflow-agent-46585987317.asia-northeast3.run.app/)).

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-03.png' | relative_url }}" alt="소상공인 대출 사전심사 심사 대시보드 — 총 심사 건수, 승인율, 온체인 집행 건수, 누적 집행액 및 최근 심사 결과 표">
  <figcaption>심사 대시보드 (Live POC)</figcaption>
</figure>

## 3. 시스템 아키텍처

### 3-1. AI Agent 판정 파이프라인

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-04.png' | relative_url }}" alt="정형 데이터 → XGBoost → Gemini+RAG → 최종 판정 → Solana devnet → BigQuery로 이어지는 판정 파이프라인">
  <figcaption>AI Agent 판정 파이프라인</figcaption>
</figure>

**핵심 설계 포인트**

- **정량·정성·정책 판단의 역할 분리** — 머신러닝의 위험 예측, LLM의 사업 맥락 해석, RAG의 정책 검색을 각각 분리해 최종 판정 근거를 추적할 수 있도록 구성
- **LLM 의사결정 범위를 프롬프트로 제한** — 정성 판단은 승인과 조건부승인 사이의 조정에만 활용하도록, 정량 모델에서 거절된 신청자를 LLM이 임의로 상향하지 못하도록 프롬프트에 규칙 명시
- **판단과 실행의 연결** — AI가 단순히 대출 여부를 추천하는 데서 끝나지 않고, 최종 판정 이후 Solana 트랜잭션까지 직접 실행하도록 구현
- **온체인 무결성 검증** — 대출 집행 트랜잭션과 판정 근거의 SHA-256 Hash를 함께 기록해 사후에 판단 근거의 변경 여부를 검증할 수 있도록 설계

### 3-2. GCP 서빙 및 자동화 인프라

- **보안**: Gemini API Key를 코드에 직접 포함하지 않고 Google Cloud Secret Manager를 통해 런타임에 안전하게 주입
- **배포**: Cloud Run Source Deploy를 활용하고, Cloud Build가 애플리케이션의 컨테이너 이미지를 자동 빌드·배포하도록 구성
- **데이터 통합**: 동기 심사, 이벤트, 스케줄 파이프라인의 결과를 하나의 BigQuery 데이터셋으로 통합해 대출 심사부터 집행 및 재심사까지 전체 이력을 조회할 수 있도록 구성

## 4. 성능 및 검증

**ML 모델 성능** — Test Dataset 37,800건 → 최종 Test AUC 0.7883

**거절 기준 성능** — Recall 35.7% / Precision 43.7% / Accuracy 86.4%

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-06.png' | relative_url }}" alt="거절 결정 기준 Confusion Matrix — TN 31,005, FP 2,145, FN 2,988, TP 1,662">
  <figcaption>Confusion Matrix (거절 결정 기준, test set)</figcaption>
</figure>

**주요 피처** — 지역 위험도, 차량 보유 여부, 사업장 소유 형태 순으로 중요도가 높았습니다.

<figure>
  <img src="{{ '/assets/images/projects/10-ai-agent-loan/img-07.png' | relative_url }}" alt="XGBoost 18개 피처 중요도 바 차트 — biz_city_risk_te, has_car, biz_premise_ownership_owned 순">
  <figcaption>Feature Importance — XGBoost (전체 18개 피처)</figcaption>
</figure>

## 5. 기술 스택 정리

| Category | Technology |
|---|---|
| ML | XGBoost(서빙) · LightGBM(비교 모델, 서빙 미포함) |
| LLM / Agent | Gemini gemini-flash-lite-latest, Function Calling |
| RAG | 자체 구현 Cosine Similarity 기반 Retrieval |
| LLM SDK | google-genai |
| Blockchain | Solana devnet |
| Token | Circle Official Devnet USDC, SPL Token |
| On-chain Proof | SPL Memo, SHA-256 |
| Solana SDK | solana-py, solders |
| Backend | FastAPI, Uvicorn, python-multipart |
| GCP | Cloud Run, BigQuery, Pub/Sub, Eventarc, Cloud Workflows, Cloud Scheduler, Cloud Build, Secret Manager |
| Language / Library | Python 3.12, Pydantic, hashlib |
