---
description: >-
  Regression, Residual Analysis, Error, Residual, SSE, SSR, SST, R-Squared,
  Adjusted R-Square, VIF
---

# Regression Analysis

### Regression

**두 변수 간의 인과 관계를 파악할 때 결정계수 구하고 회귀분석을 수행**한다.

예를 들어, **"마케팅을 많이 할수록 매출액이 높아질까?**" 질문에 대해 답하기 위해 회귀분석을 수행하면, **매출액 KPI 달성을 위해 마케팅을 몇 번 수행해야 하는지 예측**할 수 있다.

회귀분석은 4가지 가정이 있다.\
**선형성**: 종속변수 y와 독립변수 x간의 선형성이 있어야 한다.\
**독립성**: 다중 회귀 분석할 때 독립변수 x간의 상관관계가 없어야 한다. **독립성을 가진 각각의 독립 변수들이 종속변수의 분산을 설명하여야 영향력을 예측할 수 있다**.\
**등분산성**: 데이터가 분산이 같아야 한다.\
**정규성**: 잔차가 정규성을 띄어야 한다.

### Residual Analysis

모집단에서 얻는 오차는 관측될 수 없기 때문에 표본집단에서 얻은 잔차를 이용하여 분석하는데, 이것을 잔차 분석이라고 한다.

#### Error vs Residual

오차와 잔차의 차이는 모집단에서 얻은 값이면 오차, 표본집단에서 얻은 값이면 잔차이다.

오차 = 모집단의 회귀식에서 예측된 값 - 실제 관측값\
잔차 = 표본집단의 회귀식에서 예측된 값 - 실제 관측값

회귀분석에서 오차항들은 정규성(Normality), 등분산성(Homogeneity of Variance), 독립성(Independence)에 대한 가정이 필요하며 이 가정이 성립해야 회귀분석 결과가 타당한 것이다.

#### 잔차와 예측치의 산점도

잔차와 예측치의 산점도에서 부채모양이면 오차가 예측치가 커짐에 따라 커지거나 작아지는 것을 의미하므로 등분산 가정을 만족하지 못한다. 등분산 가정을 만족하지 못할 경우 Y를 로그변환하여 처리해볼 필요가 있다.

![](https://media.vlpt.us/images/ubiradio/post/11ff4a86-d8bb-4379-8d91-f1eb768fce23/image.png)

#### 잔차의 독립성

잔차의 독립성은 잔차와 관측치 순서간의 그래프 패턴을 보고 판단하며 독립성이 존재하려면 패턴이 있으면 안된다. 관측치 순서가 왼쪽에서 오른쪽을 증가함에 따라 잔차가 규칙적으로 줄어들거나, 잔차가 작은 값에서 큰 값으로 갑자기 변경되는 경우 잔차는 독립적이지 않다고 본다.

![](https://media.vlpt.us/images/ubiradio/post/edcc76c3-1da1-43c4-bf55-8d88d7cf50b4/residual\_02.jpg)

![](https://media.vlpt.us/images/ubiradio/post/273d1b7c-e3f6-455b-846d-3b677e3a25e2/residual\_01.jpg)

### SST(Total Sum of Squares)

실제값 Y의 총변동(SST)은 회귀식으로 설명 안되는 변동 SSE와 회귀식으로 설명 되는 변동 SSR 합이다.\
SST = SSE + SSR

![](https://media.vlpt.us/images/ubiradio/post/b8c3cc3a-e26b-4fe7-947e-47d74dd1b11e/sst.jpg)

#### SSE(Sum of Squares Residual Error)

회귀식으로 설명 안되는 변동이다.\
회귀식으로 예측한 값과 실제값 간의 차이 제곱합이다.\
0에 가까울 수록 좋다.

#### SSR(Sum of Squares Regression)

회귀식으로 설명되는 변동이다.\
회귀식으로 예측한 값과 실제값의 평균 간의 차이 제곱합이다.

#### R-Square (Coefficient of determination, 결정계수)

R-Squared는 0 ≤ R-Squared ≤ 1 값을 가지며, 회귀선의 설명력을 나타낸다. \
1에 가까울 수록 설명력이 좋아진다. \
1에 가까울 수록 설명력이 좋아지는 이유는 SST 총변동에서 회귀식으로 설명되는 정보의 비율이 높아질 수록 1에 가깝기 때문이다.\
총변동 중 회귀식으로 설명되는 변동의 비율이다.

R-Squared = SSR/SST\
R-Squared = 1-(SSE/SST)

데이터의 편차가 클수록 R-Squared 값은 작아진다. 실데이터는 R-Squared 값이 작은 경우가 대부분이지만 그렇다고 해서 회귀분석 결과가 믿을 수 없는 것은 아니다. 회귀선과 산포도를 보면서 회귀선을 왜곡시키는 데이터를 찾아서 제거해야 한다.

#### Adjusted R-Square

R-Squared는 회귀식에 독립변수가 추가될 수록 점점 커진다. 이 점을 보완한 것이 Adjusted R-Square 이다. 1에 가까울 수록 좋다.\
X변수의 개수(k)가 증가할 수록 Adjusted R-Square는 작아진다.\
Adjusted R-Square = 1 - (SSE/(n-k)) / (SST/(n-1))

One-hot encoding (Dummy variable)\
설명변수가 명목형 변수인 경우, One-hot encoding 으로 변환해서 모델링한다.

### Multicollinearity

다중공선성은 독립변수들이 서로 독립이 아니라 상호 상관관계가 강한 경우에 발생한다. 독립변수의 일부가 다른 독립 변수의 조합으로 표현될 수 있는 경우이다.

다중공선성을 확인하는 방법은 X변수들 간의 산점도나 상관계수를 분석해 상관성이 높은지 확인하는 방법과 VIF가 10 이상인지 확인하는 방법이 있다.\
VIF(Variance inflation Factor, 분산 팽창계수)는 X가 2개 이상일 경우, X간 상관성을 살펴볼 때 사용하며, 보통 10이상일 경우 다중공선성이 존재한다고 본다.

다중공선성을 해결하는 방법은 다중공선성이 있는 독립변수 하나를 제거하거나 PCA(Principal Component Analysis, 주성분 분석)를 통해 서로 독립인 주성분을 사용하여 회귀분석을 수행한다.

### 회귀모델의 평가지표

#### MAE(Mean Absolute Error)

잔차의 절대값에 대한 평균을 구한다.\
MAE가 작으면 모델 예측 성능이 높은 것이고, 크면 성능이 낮은 것이다. MAE가 0이면 완벽한 에측 변수이지만 거의 발생하지 않는다.

매출금액 예측에서 1000이라면 1000원을 높게 예측하는지 낮게 예측하는지 파악하기 힘들다.

![](https://media.vlpt.us/images/ubiradio/post/c1a9bded-ecdd-4da1-a6cd-42f92af06079/mae\_01.jpg)

![](https://media.vlpt.us/images/ubiradio/post/a1aaacaf-d144-449b-862e-3934a9f7cf49/mae\_02.jpg)

#### MAPE(Mean Absolute Percent Error)

실제값 대비 잔차의 절대값들의 평균 \* 100 으로 MAE를 비율(%)로 표현한 것이다. \
삼성전자 주가 예측 모델의 MAPE가 3%이고 카카오 주가 예측 모델의 MAPE가 5% 라면 삼성전자 가격을 예측하는 모델의 MAPE가 더 우수한 것으로 해석할 수 있다.

MAPE는 공식에서 알 수 있듯이 실제 값에 0이 포함될 경우 계산할 수 없다.

![](https://media.vlpt.us/images/ubiradio/post/8d720314-e400-4cc1-8f03-d662466d6b82/mape\_01.jpg)

####

![](https://media.vlpt.us/images/ubiradio/post/73f5570e-66ba-4bf5-bb0c-76c44e3e8250/mape\_02.jpg)

#### MSE(Mean Squared Error)

잔차의 제곱에 대한 평균이다. \
잔차를 제곱하기 때문에 1보다 작은 값은 더 작아지고 큰 값은 더 커지며 이상치에 더 민감하다.

![](https://media.vlpt.us/images/ubiradio/post/05730c36-c94c-4b86-82f7-d74757135057/mse\_01.jpg)

![](https://media.vlpt.us/images/ubiradio/post/791edcc7-83c0-4f17-a7db-b9e57e588d53/mse\_02.jpg)

#### RMSE(Root Mean Squared Error)

잔차의 제곱에 대한 평균 값에 루트를 씌운 값이다. \
제곱해서 루트를 취하기 때문에 MSE에 비해서 왜곡이 덜하다.

### 회귀모형 모델링 과정

* 독립변수와 종속변수의 차트를 그려본다.
* 연속형 변수를 선택하여 기초 통계량을 확인하고 편차가 크다면 줄이기 위해 로그변환해서 전후 편차가 줄었는지 확인하다. 편차가 줄었다면 정규분포에 더 가까워진 것이다.
* 기초통계와 Correlation 분석으로 상관관계를 확인한다.
* 데이터를 분할하여 회귀분석 모델링을 수행한다.
  * 다중선형회귀 분석이면, 설명변수들의 P-value를 확인하고 유의성이 낮은 변수는 제거한다. \
    예를 들어 유의수준 0.05 보다 큰 P-value를 가진 설명변수는 제거한다.
* Evaluation을 수행하고, R-Square, MAPE 등을 확인한다.
* 잔차와 예측값의 등분산성을 확인하고, 잔차와 누적확률분포의 정규성을 확인해야 한다.
  * 설명변수가 2개 이상이면 VIF를 계산하고 다중공선성을 확인한다. 상관계수가 0.9이상이거나 VIF가 10이상이면 다중공선성이 존재하는 것이며 설명변수를 하나를 제거한다. VIF가 1이면 상관관계가 없는 것이다.\
    다중 공선성은 설명변수의 통계적 유의성을 악화시킨다. 예측 정확도에 큰 영향을 주지는 않지만 해석의 품질을 저하시킨다.
* 회귀분석 결과를 해석한다.
  * 회귀분석 결과 \[설명변수]와 \[종속변수]는 음의 상관관계가 있고, \[설명변수]가 \[종속변수]에 대해 약 XX% 설명력을 가진 것을 확인했다.
* 설명력이 낮다면 설명변수를 더 발굴하여 설명력을 높일 수 있는 방안을 검토한다.

### 참고자료

[https://vitalflux.com/linear-regression-explained-python-sklearn-examples/](https://vitalflux.com/linear-regression-explained-python-sklearn-examples/)\
[https://www.dataquest.io/blog/understanding-regression-error-metrics/](https://www.dataquest.io/blog/understanding-regression-error-metrics/)

