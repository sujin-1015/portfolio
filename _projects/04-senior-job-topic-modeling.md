---
title: "토픽 모델링을 활용한 노인 일자리 정책 언론 담론 분석"
period: "2024/02/14 → 2025/03/12"
order: 5
cover: "/assets/images/projects/04-senior-job/cover.png"
badge: "단독 제1저자 논문 게재 (한국노인인력개발원 연구조사부 공동연구)"
field: ["사회", "정책"]
skills: ["Python", "R", "비정형", "텍스트마이닝", "토픽모델링", "자연어처리", "웹크롤링"]
---

## Step1. 데이터 수집

네이버 뉴스 플랫폼을 활용해 단일 키워드 '노인일자리'로 검색하고, Web Crawling 기법(Selenium, requests, BeautifulSoup 등)을 활용해 기사 링크, 제목, 본문 내용, 기자 이름, 작성일, 언론사 정보를 추출했습니다.

## Step2. 데이터 전처리

- 결측치 처리, 자료형 변환, 중복 처리 등
- 자연어 처리: 불용어 제거, KoNLPy 기반 형태소 분석

## Step3. LDA 토픽 모델링

<figure>
  <img src="{{ '/assets/images/projects/04-senior-job/img-01.png' | relative_url }}" alt="LDA 토픽 모델링 절차 — 데이터 수집/전처리, 형태소 분석, 매개변수 조정, LDA 수행, 결과 시각화">
  <figcaption>LDA 토픽 모델링 절차</figcaption>
</figure>

LDA(Latent Dirichlet Allocation)를 통해 문서의 단어 사용 패턴을 분석(gensim 활용)해 숨겨진 토픽을 식별하고 토픽별 핵심 키워드를 추출했습니다.

Coherence score(일관성 점수)를 모델 평가 지표로 선정하고 모델 최적화를 진행했습니다. (tuning 대상: num_topics, iterations, passes, eta 등)

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-02.png' | relative_url }}" alt="토픽 수(num_topics)에 따른 Coherence Score 변화">
    <figcaption>토픽 수에 따른 Coherence Score</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-03.png' | relative_url }}" alt="iterations 값에 따른 Coherence Score 변화">
    <figcaption>iterations에 따른 Coherence Score</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-04.png' | relative_url }}" alt="passes 값에 따른 Coherence Score 변화">
    <figcaption>passes에 따른 Coherence Score</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-05.png' | relative_url }}" alt="eta 값에 따른 Coherence Score 변화">
    <figcaption>eta에 따른 Coherence Score</figcaption>
  </figure>
</div>

## Step4. 시각화 | 네트워크 분석, 빈도 분석

- 토픽별로 추출된 키워드와 가중치를 기반으로 네트워크 그래프 구축 (networkx, matplotlib, Kamada-Kawai 등 활용)
- 빈도 분석 결과를 워드 클라우드로 시각화, 단어들의 TF 및 TF-IDF 값 계산 → 중요도가 높은 상위 단어 추출

<figure>
  <img src="{{ '/assets/images/projects/04-senior-job/img-06.png' | relative_url }}" alt="토픽 9개 키워드 네트워크 그래프">
  <figcaption>토픽 키워드 네트워크 그래프</figcaption>
</figure>

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-07.png' | relative_url }}" alt="TF 기반 워드클라우드">
    <figcaption>TF 기반 워드클라우드</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/04-senior-job/img-08.png' | relative_url }}" alt="TF-IDF 기반 워드클라우드">
    <figcaption>TF-IDF 기반 워드클라우드</figcaption>
  </figure>
</div>

## Step5. 토픽별 내용 분석

LDA 모델을 통해 도출된 각 토픽의 키워드를 가중치 순으로 정리해 각 토픽의 주요 주제와 논점을 파악했습니다.

| 토픽 | 주제 | 가중치 순 중요 키워드 |
|---|---|---|
| 1 | 초고령 사회의 정책 대응으로서 노인 일자리 | 인구, 필요, 문제, 세대, 고령화, 연금, 정책, 소득, 빈곤, 노동 |
| 2 | 고용 정책으로서 노인 일자리 | 고용, 취업자, 증가, 감소, 경제, 코로나, 통계청, 통계, 지난해 |
| 3 | 정치 공약 및 쟁점으로서 노인 일자리 | 대통령, 국가, 생각, 선거, 정치, 후보 |
| 4 | 예산안 관련 쟁점화 | 국회, 여야, 국민, 삭감, 대표, 예산안, 민생, 원내 |
| 5 | 빈곤 예방 정책으로서 노인 일자리 | 폐지, 조사, 생활, 하루, 수집, 평균, 시장 |
| 6 | 급여 적절성에 관한 논의 | 정부, 올해, 지급, 급여, 내년, 재정, 취약, 대상 |
| 7 | ESG(환경, 사회, 지배구조) | 운영, 센터, 창출, 활용, 참여, 환경, 업무, 서비스, 다양 |
| 8 | 노인 일자리 현장과 수행기관 | 기관, 시니어클럽, 참여자, 공익, 지원사업, 수행 |
| 9 | 지역 복지 정책으로서 노인 일자리 | 지역, 추진, 복지, 조성, 예산, 계획, 공공, 분야, 구축, 확대 |

키워드 간의 의미적 연관성과 맥락을 종합적으로 고려해 각 토픽의 주제를 해석하고 주제명을 부여했으며, 토픽별 대표 기사를 선정해 내용 분석을 진행했습니다. 이 연구는 한국노인인력개발원 연구조사부와의 공동연구로 이어져 단독 제1저자 논문으로 게재되었습니다.
