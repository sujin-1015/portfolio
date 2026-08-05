---
title: "이커머스 상품 별점 예측 및 추천시스템 구현"
period: "2023/06/27 → 2023/08/24"
order: 5
cover: "/assets/images/projects/05-ecommerce/cover.png"
field: ["이커머스", "상품"]
skills: ["Python", "웹크롤링", "추천시스템", "예측"]
---

## Step1. 데이터 수집

올리브영 스킨케어 제품의 다양한 정보를 크롤링했습니다.

- **제품 정보**: 브랜드명, 상품명, 카테고리, 정가, 할인가
- **고객 및 리뷰 정보**: 아이디, 별점, 피부타입, 피부고민 등

selenium, ChromeDriverManager, requests, BeautifulSoup, openpyxl 등을 활용했습니다.

<figure>
  <img src="/assets/images/projects/05-ecommerce/img-01.png" alt="크롤링한 올리브영 스킨케어 제품 및 리뷰 데이터 샘플 표">
  <figcaption>크롤링 데이터 샘플</figcaption>
</figure>

## Step2. 상품 별점 예측

Random Forest 및 KNN(K-Nearest Neighbor) 모델을 사용했고, SMOTE 오버샘플링, StandardScaler, Grid Search 등을 활용했습니다.

- 데이터 불균형 확인 → SMOTE 오버샘플링 수행

<figure>
  <img src="/assets/images/projects/05-ecommerce/img-02.png" alt="별점별 리뷰 수 분포 — 5점에 편중된 데이터 불균형">
  <figcaption>별점 분포 (데이터 불균형 확인)</figcaption>
</figure>

- [랜덤포레스트] 피처 중요도 시각화
- [랜덤포레스트] 혼동행렬 시각화

<div class="fig-row">
  <figure>
    <img src="/assets/images/projects/05-ecommerce/img-03.png" alt="랜덤포레스트 피처 중요도 Top 20 바 차트">
    <figcaption>피처 중요도 Top 20</figcaption>
  </figure>
  <figure>
    <img src="/assets/images/projects/05-ecommerce/img-04.png" alt="랜덤포레스트 혼동행렬 히트맵">
    <figcaption>혼동행렬</figcaption>
  </figure>
</div>

## Step3. 추천시스템 구현

고객-제품 평점 행렬을 생성하고, 고객 간 유사도를 `cosine_similarity`로 분석했습니다.

```python
# 고객 간 유사도 계산
from sklearn.metrics.pairwise import cosine_similarity

cos_matrix = cosine_similarity(df_users)
df_users_cosine = pd.DataFrame(cos_matrix, index=df_users.index, columns=df_users.index)
```

<figure>
  <img src="/assets/images/projects/05-ecommerce/img-05.png" alt="고객 간 코사인 유사도 행렬 출력 결과">
  <figcaption>고객 간 코사인 유사도 행렬</figcaption>
</figure>

제품 및 별점 데이터를 기반으로 User-Based CF 추천시스템을 구현했습니다. 고객 아이디와 원하는 제품 유형을 입력하면, 해당 유저와 유사도가 높은 고객이 만족한 제품을 추천합니다.

```python
def user_based_recommend(user_id, product_type):
    # 유저 아이디와 유사도 높은 5명 찾기
    sim_users = df_users_cosine[user_id].sort_values(ascending=False)[1:6].index

    # 유사한 유저들의 데이터 추출
    sim_user_df = df[df['user_id'].isin(sim_users)]

    # 입력한 product_type과 일치하는 제품 추출 및 정렬
    products = sim_user_df[sim_user_df['product_type'] == product_type].sort_values(by='rating', ascending=False)

    # rating 4점 이상인 제품만 추출
    products = products[products['rating'] >= 4.0]

    # product_name 중복 제거 (첫 번째 값만 남김)
    products.drop_duplicates(subset='product_name', keep='first', inplace=True)

    result = pd.DataFrame(products['product_name'])
    result.columns = ['나와 비슷한 사용자가 만족한 ' + product_type + ' 제품']
    return result

result = user_based_recommend(user_id=1, product_type='앰플')
```

<figure>
  <img src="/assets/images/projects/05-ecommerce/img-06.png" alt="user_id=1, product_type='앰플'에 대한 추천 결과 출력">
  <figcaption>추천 결과 예시 (앰플 제품)</figcaption>
</figure>

**추천 로직 요약**: 입력 아이디와 유사도 높은 5명 탐색 → 해당 유저들의 데이터 중 원하는 product_type 필터링 → rating 내림차순 정렬 → rating 4점 이상만 → 상품명 중복 제거 → 추천 결과 반환
