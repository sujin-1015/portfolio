---
title: "MLB 경기 점수 및 승패 예측 모델링"
period: "2025/04/20 → 2025/06/18"
order: 9
cover: "/assets/images/projects/08-mlb/cover.png"
field: ["스포츠", "야구"]
skills: ["R", "Python", "모델링", "예측"]
---

## 프로젝트 목표

데이터 기반의 MLB 경기 결과 예측을 넘어, 승패 예측을 넘어선 정량적인 점수 예측과 머신러닝 모델의 실효성 검증을 목표로 했습니다.

## Step1. 데이터 수집 및 전처리

R의 `baseballR` 패키지를 활용해, 총 7개 팀의 MLB 정규 시즌 경기 데이터를 수집했습니다. (팀 정보 13개 + 타자 정보 7개 + 선발투수 정보 24개, 총 44개 변수 확보)

**수집 범위**: 2024년 경기 162개 + 2025년 5월 22일까지의 경기 (시범 경기 및 포스트 시즌 제외)

**대상 팀**: San Francisco Giants, Seattle Mariners, Miami Marlins, Detroit Tigers, Houston Astros, Minnesota Twins, Washington Nationals

<figure>
  <img src="{{ '/assets/images/projects/08-mlb/img-01.png' | relative_url }}" alt="분석 대상 7개 MLB 팀 로고 — Giants, Mariners, Nationals, Marlins, Tigers, Astros, Twins">
  <figcaption>분석 대상 7개 팀</figcaption>
</figure>

데이터 전처리로 타순 가중 평균 처리, 범주형 변수 처리, 결측치 처리 등을 진행했습니다.

| 구분 | 주요 변수 |
|---|---|
| 팀 정보 | 경기 고유 ID, 경기 날짜, 홈 경기 여부, 득점, 실점, 팀 홈런 수, 팀 타율, 팀 출루율 등 |
| 타자 정보 | 타율/OBP/SLG/OPS 타순가중평균, 득점권 타율 타순가중평균 등 |
| 선발투수 정보 | 투수 고유 ID, 선발 투수 이름, 평균 자책점, 이닝당 볼넷+안타 허용률, 수비 무관 투구, 좌투/우투, 상대 전적 ERA 등 |

<figure>
  <img src="{{ '/assets/images/projects/08-mlb/img-02.png' | relative_url }}" alt="수집된 경기별 팀·타자·투수 데이터 샘플 데이터프레임">
  <figcaption>수집 데이터 샘플</figcaption>
</figure>

## Step2. 모델링

LightGBM Regression, XGBoost Regression을 사용했고, Optuna와 GridSearchCV로 모델을 최적화했습니다.

**평가 기준**: RMSE, MAE, R-squared, Adjusted R-squared

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/08-mlb/img-03.png' | relative_url }}" alt="Minnesota Twins XGBoost 피처 중요도 바 차트">
    <figcaption>피처 중요도 — Minnesota Twins</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/08-mlb/img-04.png' | relative_url }}" alt="Seattle Mariners XGBoost 피처 중요도 바 차트">
    <figcaption>피처 중요도 — Seattle Mariners</figcaption>
  </figure>
</div>

두 팀 모두 팀 OPS(ops.team)가 가장 중요한 예측 변수로 나타났으며, 출루율·장타율·타율 등 팀 타격 지표와 상대 선발투수의 볼넷·삼진 관련 지표가 뒤를 이었습니다.

## Step3. 앙상블 및 예측

LightGBM과 XGBoost 모델을 6:4로 Ensemble했습니다.

```
0.6 * LightGBM + 0.4 * XGBoost
```

두 모델의 예측 점수를 가중 평균해, 예측된 점수를 기준으로 승패 여부를 결정했습니다.

<figure>
  <img src="{{ '/assets/images/projects/08-mlb/img-05.png' | relative_url }}" alt="예측 점수가 더 높은 팀을 승자로 판정하는 로직 다이어그램">
  <figcaption>승패 판정 로직</figcaption>
</figure>

팀별로 XGBoost·LightGBM·Ensemble 세 모델의 실제 경기 결과 대비 예측 점수를 비교한 결과, 두 팀 모두 RMSE·MAE가 낮고 R²가 0.9 이상으로 높은 정확도를 보였습니다.

<div class="fig-row">
  <figure>
    <img src="{{ '/assets/images/projects/08-mlb/img-06.png' | relative_url }}" alt="San Francisco Giants 모델별 예측 결과 및 RMSE/MAE/R²/Adj.R² 비교표">
    <figcaption>모델 비교 — San Francisco Giants</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/projects/08-mlb/img-07.png' | relative_url }}" alt="Seattle Mariners 모델별 예측 결과 및 RMSE/MAE/R²/Adj.R² 비교표">
    <figcaption>모델 비교 — Seattle Mariners</figcaption>
  </figure>
</div>

두 팀 모두 스코어 예측 결과와 성능지표를 종합했을 때 **XGBoost 모델이 가장 우수**한 것으로 나타났습니다.
