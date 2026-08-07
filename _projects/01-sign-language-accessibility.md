---
title: "AI 기반 양방향 수어 대화 및 접근성 가전 제어 서비스"
period: "2026/05/18 → 2026/06/25"
order: 1
cover: "/assets/images/projects/01-sign-language/cover.png"
badge: "LG전자 DX School 최종 프로젝트 우수상 수상"
field: ["AI", "접근성", "기업", "고객"]
skills: ["AI", "딥러닝", "모델링", "Python", "JavaScript", "PyTorch", "프런트/백엔드", "데이터분석"]
---

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/01-sign-language/img-01.jpg' | relative_url }}" alt="LG전자 DX School 지능형 고객경험데이터 분석 서비스 프로젝트 우수상 인증서">
    <figcaption>LG전자 DX School 5기 우수상 인증서</figcaption>
  </figure>
  <div class="fig-stack">
    <figure>
      <img src="{{ '/assets/images/projects/01-sign-language/img-02.png' | relative_url }}" alt="현장 시연 사진">
      <figcaption>현장 시연</figcaption>
    </figure>
    <figure>
      <img src="{{ '/assets/images/projects/01-sign-language/img-03.png' | relative_url }}" alt="팀 단체 사진">
      <figcaption>팀 단체 사진</figcaption>
    </figure>
  </div>
</div>

## 프로젝트 추진 배경

장애 종류별 인구 비율 및 글로벌 보조기기 시장 전망을 볼 때, 국내 등록장애인 중 지체·청각·시각 장애 비중이 높고, 글로벌 보조기기 시장은 연평균 8.12% 고성장이 전망됩니다 (2035년 약 52조 원 규모). 2025 유럽 접근성법(EAA), 2026 국내 장애인차별금지법 개정으로 가전제품 접근성 보장이 의무화되는 흐름 속에서, LG전자는 다양한 가전을 연결하는 ThinQ 생태계와 카메라·이동형 디스플레이를 갖춘 스탠바이미를 보유하고 있어 음성·수어·시각 인식 UI를 하나의 허브에 자연스럽게 통합할 수 있는 유일한 위치에 있습니다.

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-04.png' | relative_url }}" alt="장애 종류별 인구 비율 및 글로벌 보조기기 시장 전망 차트">
  <figcaption>장애 종류별 인구 비율 및 글로벌 보조기기 시장 전망</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-05.png' | relative_url }}" alt="LG전자의 강점 — 스탠바이미, ThinQ, SW 업데이트">
  <figcaption>LG전자의 강점</figcaption>
</figure>

## 1. 핵심 고객 경험 설정

**하드웨어를 넘어 생활 경험으로.** 소프트웨어 접근성 기반 AI 솔루션으로 보조기기 시장을 일상 경험 중심으로 확장합니다.

청각·시각·지체 장애 당사자들의 실제 리뷰를 크롤링 및 분석해 3가지 핵심 니즈를 도출했습니다.

- **사람들과 원활하게 대화하고 싶다 (청각)** — 여러 사람이 동시에 말할 때 대화를 따라가기 어려움
- **타인의 도움 없이 스스로 자립하고 싶다 (시각)** — 가전 조작을 위해 매번 도움을 요청해야 함
- **누군가의 도움 없이 이동하고 싶다 (지체)** — 리모컨·작은 화면 조작이 불편함

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-06.png' | relative_url }}" alt="청각·시각·지체 장애 당사자 리뷰 및 Pain Point 분석">
  <figcaption>실제 리뷰 기반 Pain Point 분석</figcaption>
</figure>

### 고객 감동 목표 설정

보조기기를 넘어, 더 능동적인 일상으로 — LG의 디바이스 생태계를 활용해 장애인의 일상 속 제약을 줄이고 스스로 살아갈 수 있는 경험을 제공합니다.

- **GOAL 1. 개인화 강화** : 장애 유형, 보조기기, 생활환경에 맞춘 맞춤형 서비스 제공
- **GOAL 2. 독립성 강화** : 타인의 도움 없이 소통하고 가전을 직접 조작할 수 있도록 지원
- **GOAL 3. 안전 강화** : 가전 알림, 비상 상황 정보를 장애 유형에 맞게 전달
- **GOAL 4. 비즈니스 확장** : B2C 가정용 서비스를 넘어 특수학교·복지관·병원·공공기관 등으로 확장

## 2. 서비스 기획 — LG DoDo

> 말이 필요 없는 대화, 사람과 사람 그리고 사람과 가전을 잇다.

- 실시간 수어·음성 양방향 변환 및 가전 제어
- 사용자의 장애 유형 기반 맞춤형 UI 및 알림 지원
- 대화 내용 저장 및 요약 리포트 제공

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-07.png' | relative_url }}" alt="LG DoDo 서비스 정의서 — Target, Concept, Touch Point, Key Value">
  <figcaption>서비스 정의서</figcaption>
</figure>

**핵심 가치**

- 독립성: 타인의 도움 없이 가전 상태 확인 및 제어
- 소통의 질 향상: 그룹 대화의 발화자와 맥락을 실시간 파악
- 접근성 강화: 장애 유형에 맞는 맞춤형 UI

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-08.png' | relative_url }}" alt="LG DoDo 서비스 구조도 — 수어 대화, 가전 제어, 마이페이지">
  <figcaption>LG DoDo 서비스 구조도</figcaption>
</figure>

## 3. 서비스 구현

### 3-1. AI 수어 대화

- **수어 인식** : [수어 → 텍스트] ST-GCN
- **수어 생성** : [음성 → 텍스트 → 수어] STT + Transformer Decoder (Python·PyTorch)
- 생성된 수어는 3D VRM 아바타(Three.js + Kalidokit)가 실시간 모션으로 재현
- 음성 입력부터 아바타 렌더링까지 실시간으로 이어지는 데이터 파이프라인을 FastAPI + WebSocket으로 구축
- **다중 화자 식별** : Resemblyzer 화자 임베딩 + K-means 클러스터링 + 코사인 유사도 매칭
- 대화 시작 전 참여자별 음성 등록(Voice Enrollment) 절차를 거쳐, 그룹 대화 시 다중 화자 식별 정확도 확보
- AI가 그룹 대화 맥락을 분석해 핵심 요약 및 해시태그 기반 리포트 저장

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-09.png' | relative_url }}" alt="AI 수어 대화 화면 — 실시간 대화 및 대화 리포트">
  <figcaption>실시간 대화 &amp; 대화 리포트 화면</figcaption>
</figure>

### 3-2. AI Agent — 대화 맥락 기반 가전 제어 제안

- 대화 중 "덥다", "TV 틀어줘" 같은 발화를 LLM이 실시간으로 감지해 의도(기기·동작)를 분류 (에어컨·TV·조명·로봇청소기·공기청정기)
- 의도가 감지되면 "에어컨을 켤까요?" 형태의 팝업을 띄우고, "예" 선택 시 실행 완료 메시지까지 자동 응답
- 수어 문장뿐 아니라 음성도 함께 모니터링해 대화 흐름 속에서 자연스럽게 가전 제어로 연결
- LLM 응답 실패 시 키워드 매칭 기반 로직으로 자동 전환(Fallback)해 서비스 끊김 방지
- 동일 팝업이 반복 노출되지 않도록 15초 쿨다운 적용

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/01-sign-language/img-10.png' | relative_url }}" alt="가전 제어 제안 팝업 예시">
    <figcaption>제어 제안 팝업</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/01-sign-language/img-11.png' | relative_url }}" alt="가전 제어 완료 메시지">
    <figcaption>실행 완료 메시지</figcaption>
  </figure>
</div>

### 3-3. 손쉬운 가전 제어 — ThinQ 연동

- 통합 대시보드에서 세탁기·에어컨·TV·전기레인지·공기청정기·로봇청소기 등 LG 가전을 스탠바이미 하나로 제어
- 기기별 상세 제어 화면 구현 (예: 전기레인지 화구별 사용 상태 표시)
- 작동 완료·오류 상황을 시각/진동 등 맞춤형 알림으로 안내
- 저시력자를 위한 큰 버튼 및 고대비 인터페이스

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-12.png' | relative_url }}" alt="ThinQ 연동 통합 대시보드, 알림, 에어컨 제어 화면">
  <figcaption>ThinQ 연동 가전 제어 화면</figcaption>
</figure>

### 3-4. 마이페이지 — 개인화 접근성 설정

- 음성 알림 / 큰 글씨 / 고대비 / 색상 보정 모드 지원
- 사용자의 장애 유형에 맞춰 원클릭 커스터마이징
- 비상 연락망 등록으로 위급 상황 대비

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-13.png' | relative_url }}" alt="마이페이지 접근성 환경 설정 화면">
  <figcaption>개인화 접근성 환경 설정</figcaption>
</figure>

## 4. 기대 효과

그룹 대화 이해도 및 가전 제어 자립도 상승과 함께, LG 비즈니스 관점에서도 접근성 킬러앱으로 스탠바이미 구매를 유도하고 DoDo 허브를 통한 THINQ 가전 생태계를 확장하며, EU 접근성법(EAA) 대응 글로벌 경쟁력을 확보할 수 있습니다.

등록 청각장애인 44.9만 명·시각장애인 24.5만 명에 고령화에 따른 감각장애 인구 증가를 더하면, B2G(복지관·특수학교 등)로 확장 가능한 시장성을 갖추고 있습니다.

<figure>
  <img src="{{ '/assets/images/projects/01-sign-language/img-14.png' | relative_url }}" alt="정량적 분석, LG 비즈니스 가치, 고객 가치 요약">
  <figcaption>기대 효과 요약</figcaption>
</figure>

## 기술 스택 정리

| 영역 | 내용 |
|---|---|
| 데이터 | Reddit, 네이버 블로그/카페/지식인, 유튜브, 스마트스토어 등 웹 크롤링 → 약 15만 건 데이터 수집 · BERTopic·LDA 토픽모델링 · 수어 영상 데이터 → MediaPipe Holistic으로 관절 좌표 추출 · 임베딩+PCA+K-means 클러스터링 |
| AI/모델링 | ST-GCN → 수어 인식 · Transformer Decoder → 수어 생성 · STT API (CLOVA Speech) · Resemblyzer 화자 임베딩 + K-means + 코사인 유사도 → 다중 화자 식별 · LLM 기반 가전제어 Agent |
| UI/UX 설계 | Figma 기반 화면설계 및 프로토타이핑 |
| 프론트엔드 시각화 | Three.js + Kalidokit 기반 3D VRM 아바타 수어 렌더링 · MediaPipe Holistic 실시간 랜드마크 추출 |
| 개발 | FastAPI + WebSocket 기반 실시간 통신 아키텍처 구축 · JavaScript 기반 프론트엔드 구현 · AI Agent·인식 파이프라인 백엔드 구현 · 시스템 아키텍처 설계 |
