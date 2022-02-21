---
description: Ensemble Learning, Decision Tree, Bootstrap, Bagging, Boosting
---

# Ensemble Learning

앙상블 학습은 동일한 학습 알고리즘을 사용해서 **여러 모델을 학습한 후 그 결과를 조합하는 학습 기법**이다.\
앙상블 기법을 사용하면 과적합을 감소시키고, 단일 모델 보다 예측 정확도를 높일 수 있다.

모델의 예측결과에서 **예측값과 실제값이 멀리 떨어져 있으면 결과의 편향(bias)이 높고**, **예측값들 끼리 멀리 흩어져 있으면 분산(variance)이 높다**고 한다. 이렇듯, 학습 오류의 주요 원인이 편향, 분산 때문인데 앙상블은 이러한 요소를 최소화할 수 있다.

![](https://media.vlpt.us/images/ubiradio/post/af4cc3c0-6604-47b7-8338-f53d89b3c3fd/image%20\(1\).jpg)

앙상블 학습 기법에는 배깅(Bagging)과 부스팅(Boosting)이 있다. 먼저, 결정트리(Decision Tree)와 부트스트랩(Bootstrap)에 대해 알아야 한다.

#### 결정트리

결정트리는 트리구조로 데이터를 각 결정 규칙에 따라 분할하고 최종적으로 어떤 범주에 해당하는지 결정하는 방법이다.

#### 부트스트랩

부트스트랩은 resampling 방벙 중 하나로 모집단의 분포를 모르거나 샘플이 부족한 경우에 데이터를 랜덤 샘플링으로 복원추출하여 학습데이터를 늘리는 방법이다. 부트스트랩을 사용하면 분포를 고르게 만드는 효과가 있다. 참고로, 다른 resampling 방법으로는 K-Fold Cross Validation 이 있다.

![](https://media.vlpt.us/images/ubiradio/post/8732a2c9-55d2-4a76-8dfc-2f2f68b06902/bootstrap.jpg)

#### Bagging

배깅은 각 모델이 서로 독립이고 병렬로 학습한다. 샘플을 여러 번 뽑아(Bootstrap) 여러 개의 독립적인 결정트리가 예측 모델을 생성한 후, 그 결과물을 집계(Aggregation)해 최종 모델을 만드는 방법이다. 분류 문제는 투표 방식(Voting)으로 결과를 집계하며, 회귀 문제는 평균 혹은 중앙값으로 집계한다. 대표적인 배깅 기법을 활용한 알고리즘이 **Random Forest**이다.

#### Boosting

배깅은 병렬로 학습하고 부스팅은 순차적으로 학습한다. 이전 모델로 예측한 결과 중에 잘못 분류한 데이터에 가중치를 반영해서 다음 모델에 전달한다. 다음 모델은 학습할 때 해당 데이터에 더 집중하여 새로운 분류 규칙을 만드는데, 이 단계를 반복한다. 대표적인 부스팅 알고리즘으로 **Gradient Boosting 있고 이 알고리즘으로 병렬 학습 가능한 알고리즘이 XGBoost이다.**

#### XGBoost (Extreme Gradient Boosting)

Gradient Boosting 알고리즘을 분산환경에서 실행할 수 있도록 구현한 알고리즘이다.

![](https://media.vlpt.us/images/ubiradio/post/1a9a3cbf-da19-4de1-ad36-25db524cdd14/bagging\_boosting.jpg)

### 참고자료

[https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/](https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/)\
[https://becominghuman.ai/ensemble-learning-bagging-and-boosting-d20f38be9b1e](https://becominghuman.ai/ensemble-learning-bagging-and-boosting-d20f38be9b1e)\
[https://m.blog.naver.com/yjhead/222116788833](https://m.blog.naver.com/yjhead/222116788833)\
[https://opentutorials.org/module/3653/22071](https://opentutorials.org/module/3653/22071)\
[https://algotech.netlify.app/blog/xgboost/](https://algotech.netlify.app/blog/xgboost/)\
[https://bcho.tistory.com/1354](https://bcho.tistory.com/1354)\
[https://modern-manual.tistory.com/31](https://modern-manual.tistory.com/31)

