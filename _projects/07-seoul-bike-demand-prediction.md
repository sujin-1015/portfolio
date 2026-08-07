---
title: "서울시 공공자전거 수요 예측 및 영향 요인 분석"
period: "2023/11/13 → 2023/12/07"
order: 8
cover: "/assets/images/projects/07-seoul-bike/cover.png"
field: ["교통", "공공서비스"]
skills: ["Python", "예측", "회귀", "모델링"]
---

## Step1. 데이터 수집

총 5개 범주의 데이터를 식별키를 기준으로 통합했습니다.

- 공공 자전거: 이용건수 등
- 집객시설 및 상권
- 아파트: 평균 시가 등
- 교통: 버스정거장 수 등
- 인구

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-01.png' | relative_url }}" alt="행정동별로 통합된 공공자전거·상권·집객시설·인구 데이터 스프레드시트">
  <figcaption>행정동 기준 통합 데이터 샘플</figcaption>
</figure>

## Step2. EDA 및 데이터 전처리

자치구별 현황을 분석하고, 변수 분포를 확인해 표준화/정규화 필요성을 파악했습니다. 변수 간의 상관관계를 살펴 '공공자전거 이용건수'와 상관관계가 높은 변수를 파악했습니다.

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-02.png' | relative_url }}" alt="자치구별 이용건수 및 대여소개수 막대그래프">
  <figcaption>자치구별 이용건수 · 대여소개수 현황</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-03.png' | relative_url }}" alt="자치구별 이용건수 분포 산점도">
  <figcaption>자치구별 이용건수 분포</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-04.png' | relative_url }}" alt="이용건수, 상권 수, 집객시설 수 등 주요 변수의 히스토그램 분포">
  <figcaption>주요 변수 분포 (표준화/정규화 필요성 확인)</figcaption>
</figure>

**데이터 전처리**: 변수명 변경, 결측치 처리, 중복 처리, 이상치 처리, log 변환, 표준화 등

## Step3. 이용 패턴 분석

Linear Regression을 사용했고, Standard / MinMax / Robust Scaler, Log Transformation, Cook's Distance 등을 활용했습니다. R-Squared 값을 기준으로 모델을 선택했습니다.

**회귀진단**: 다중공선성(VIF), 정규성(Q-Q plot), 등분산성(Equal Variance Check)을 확인하고 결과를 해석해 활용 방안을 제시했습니다.

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-05.png' | relative_url }}" alt="Normal Q-Q plot, Equal Variance Check, 변수별 VIF 표">
  <figcaption>회귀진단 — 정규성 · 등분산성 · 다중공선성(VIF)</figcaption>
</figure>

Robust Scaler로 스케일링한 데이터셋으로 도출한 최종 회귀식은 다음과 같습니다.

```
y = 0.44x1 + 0.49x2 - 0.37x3 - 0.18x4 + 0.73x5 - 0.07x6 + 0.12x7 + 0.18
(y = 공공자전거 이용건수, x1 = 남자 40대 생활인구, x2 = 30대 한국인 수, x3 = 아파트단지 수,
 x4 = 길단위 유동인구, x5 = 초등학교 수, x6 = 직장인구, x7 = 지하철 하차 승객 수)
```

남자 40대 생활인구, 30대 한국인 수, 아파트단지 수, 지하철 하차 승객 수 등 회귀계수가 큰 변수일수록 이용건수(종속변수)에 큰 영향을 미쳤습니다. 이 중 남자 40대 생활인구가 2017~2023년 동안 꾸준히 증가한 청룡동·청룡동 등의 행정동을, 공공자전거 추가 설치 및 재배치 장소 선정에 활용할 수 있습니다.

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-06.png' | relative_url }}" alt="최종 회귀식, 변수 설명, 남자 40대 생활인구 연도별 추이 그래프">
  <figcaption>최종 회귀식 및 활용 방안</figcaption>
</figure>

## Step4. 타 지역 설치 방향 제시

Logistic Regression을 사용했고, Forward Selection, Hosmer-Lemeshow test 등을 활용했습니다. 서울시 이외의 지역에서도 변수들이 서울시와 유사한 경향성을 보인다면 공공자전거 유치 계획 수립에 활용할 수 있습니다. (예시: 20대 한국인 수와 이용건수는 양의 상관관계 → 20대 한국인이 많은 행정동 위주로 유치 시 초기 손실 예방 가능)

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-07.png' | relative_url }}" alt="지방 공공자전거 유치 방향성 제시 다이어그램">
  <figcaption>지방 공공자전거 유치 방향성 제시</figcaption>
</figure>

## 분석 의의

선형회귀분석의 설명력(R²)은 0.46에 그쳤으나, 사회과학 데이터에서는 통상 0.3 이상을 유의미한 결과로 판단하는 점을 고려하면 현상에 대한 어느 정도의 설명력을 확보했다고 볼 수 있습니다. 이후 변동하는 X 변수를 기준으로 공공자전거 배치 행정동을 선별하거나, 시계열 분석과 결합해 시기별 이용량을 예측하는 데 활용할 수 있습니다. 로지스틱 회귀분석을 통해서는 이용건수가 많은 지역과 그렇지 않은 지역에 영향을 미치는 변수를 추출해, 지방 공공자전거 유치 방향성을 제시할 수 있습니다. 공공자전거 이용 활성화는 궁극적으로 탄소 배출량 및 교통 체증 감소로 이어집니다.

<figure>
  <img src="{{ '/assets/images/projects/07-seoul-bike/img-08.png' | relative_url }}" alt="공공자전거 이용 활성화의 분석 의의 — 선형회귀분석과 로지스틱 회귀분석 요약">
  <figcaption>분석 의의 요약</figcaption>
</figure>
