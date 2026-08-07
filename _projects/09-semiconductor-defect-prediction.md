---
title: "반도체 제조 공정 데이터 기반 불량률 예측 및 원인 분석"
period: "2025/04/20 → 2025/06/10"
order: 10
cover: "/assets/images/projects/09-semiconductor/cover.jpg"
field: ["제조", "반도체공정"]
skills: ["Python", "예측", "회귀"]
---

## Step1. 데이터 수집

삼성SDS Brightics AI 포털에서 제공하는 데이터를 사용했습니다.

- 수집 기간: 2008.07.19 ~ 2008.10.17
- 1,567개의 샘플, 592개의 변수
- 반도체 공정 내 센서 데이터 590개 (Sensor0 ~ Sensor589)
- 품질 판정값(Pass_Fail): -1이면 합격 & 1이면 불량
- 측정 시각 정보(SensorTime)

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-01.png' | relative_url }}" alt="SensorTime, Sensor0~Sensor589, Pass_Fail 컬럼을 포함한 원시 데이터 샘플">
  <figcaption>원시 센서 데이터 샘플</figcaption>
</figure>

## Step2. EDA 및 데이터 전처리

Skewness(왜도) 분석을 진행하고 불량 분포를 확인했습니다.

| 구분 | 비율 | 수 |
|---|---|---|
| 양품 | 0.9336 | 1,463 |
| 불량 | 0.0664 | 104 |

결측치 비율 상위 15개 센서를 확인하고, 이상치 비율이 높은 센서를 시각화했으며, 상관관계 분석을 진행했습니다.

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-02.png' | relative_url }}" alt="결측치 비율이 높은 상위 15개 센서 막대그래프">
  <figcaption>결측치 비율 상위 15개 센서</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-03.png' | relative_url }}" alt="Sensor31, Sensor40 이상치 분포 박스플롯">
  <figcaption>이상치 비율이 높은 센서 시각화</figcaption>
</figure>

**데이터 전처리**

- Skewness 보정
- Z-score normalization
- 이상치 처리 (처리 전/후 비교)

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-04.png' | relative_url }}" alt="이상치 처리 전후 박스플롯 비교">
  <figcaption>이상치 처리 전/후 비교</figcaption>
</figure>

- 결측치 처리: 변수 제거 및 중앙값 처리 (결측률 상위 40개 변수 및 cutoff 기준 적용)

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-05.png' | relative_url }}" alt="결측률 상위 40개 변수와 45% cutoff 기준선 막대그래프">
  <figcaption>결측률 상위 40개 변수 및 cutoff 기준</figcaption>
</figure>

## Step3. 모델링 및 불량 탐지 성능 평가

**Random Forest** — SMOTE Oversampling, Threshold tuning 적용, ROC 곡선·PR 곡선·피처 중요도 시각화

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-06.png' | relative_url }}" alt="랜덤포레스트 모델의 ROC 곡선(AUC 0.7411), Precision-Recall 곡선, Top 20 피처 중요도">
  <figcaption>랜덤포레스트 — ROC·PR 곡선 및 피처 중요도</figcaption>
</figure>

**Logistic Regression** — liblinear solver, StandardScaler, SMOTE Oversampling, Threshold tuning 적용, F1-score/Recall/Precision/ROC·PR curve로 평가

<figure>
  <img src="{{ '/assets/images/projects/09-semiconductor/img-07.png' | relative_url }}" alt="로지스틱 회귀 모델의 ROC 곡선(AUC 0.6134), Precision-Recall 곡선, Top 20 피처 계수">
  <figcaption>로지스틱 회귀 — ROC·PR 곡선 및 피처 계수</figcaption>
</figure>

### 모델 비교

| 성능 평가 지표 | Accuracy | Precision | Recall | F1-score | AUC score |
|---|---|---|---|---|---|
| 랜덤포레스트 | 0.7102 | 0.1354 | 0.6190 | 0.2222 | 0.7411 |
| 로지스틱 회귀 | 0.8503 | 0.1389 | 0.2381 | 0.1754 | 0.6134 |

Recall과 AUC 기준으로는 랜덤포레스트가, Accuracy 기준으로는 로지스틱 회귀가 우수해 불량 탐지 목적에 맞춰 임계값과 모델을 함께 고려했습니다.
