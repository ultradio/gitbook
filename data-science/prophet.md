---
description: Prophet
---

# Time Series Analysis

## Prophet을 이용한 시계열 분석 및 예측

시계열 분석은 생산관리나 수요예측 등 실무에서 많이 필요로 하지만 품질이 그다지 좋지 않는 편이다. 제품 및 서비스에 대한 수요를 더 잘 예측하기 위해 시계열 분석의 속도와 정확성을 개선하는 것이 매우 중요하다.\
예를 들어 제품이 매장에 적게 배치되면 고객은 필요할 때 제품을 구매할 수 없어서 소매업체의 수익 손실을 초래하고 소비자들은 불만으로 고객이 경쟁업체로 이동할 수 있다.\
따라서 정확하고 시기적절한 예측이 중요하다.

### Prophet

Prophet은 페이스북에서 공개한 시계열 분석 및 예측 알고리즘이며, 최대한 많은 사람들이 쉽게 사용할 수 있는 시계열 도구를 제공하는 것이 목적이다. 정확도가 높고 직관적인 파라미터를 조정해 모델을 튜닝할 수 있다. 계절성이 있고 여러 시즌의 과거 데이터가 있는 시계열에서 잘 동작하며, KPI 예측에 활용할 수 있다.

Prophet은 다음과 같은 데이터에 특히 유용하다.

시간의 흐름에 따라 관측된 시계열 데이터로 연 단위 이상이면 좋다.\
계절적 특성이 존재한다.\
불규칙적으로 일어나지만 사전에 시점을 알고 있는 이벤트가 있는 데이터를 포함하고 있다.\
예를들어, 블랙 프라이데이는 특정한 이벤트로 인해 장기적인 추이가 변할 수 있다. 신제품 출시, 디자인 변경은 비선형 성장 추세이다.\
증가할 수 있는 지표의 최대치가 존재하고 이를 알고 있는 경우이다.\
결측치가 존재하거나 이상치가 많다.

Prophet의 주요 구성요소는 Growth, Seasonality, Holidays(Events), Error Term 이며, 공식은 다음과 같다.

![](https://media.vlpt.us/images/ubiradio/post/1cf3f852-7af3-4686-8874-2d5f2edb9568/prophet\_algo.jpg)

### **Growth: g(t)**

Growth는 **시간에 따라 추세(Trend)**를 나타내며, Linear Growth (Change Point)와 Non-Linear Growth (Logistic Growth), Flat Growth 가 있다.

#### Logistic Growth Model

Logistic Growth model은 자연적인 상한선이 존재하는 경우 사용한다.

![](https://media.vlpt.us/images/ubiradio/post/25538341-461a-4ee6-bd8f-f4c671fa1383/growth.JPG)

C (Capacity) 상한선, k 는 growth rate로 곡선의 기울기, m이 offset parameter 이다.

![](https://media.vlpt.us/images/ubiradio/post/4a19af3f-1b7b-49bb-aa59-6ccc9fbab758/prophet\_c.png)

지수 모델은 단기적으로 유용할 수 있지만 성장이 영원히 지속될 수 없기 때문에 오래 지속되면 무너지는 경향이 있다. Capacity와 k(성장률)가 시간에 따라 변할 수도 있다.

#### Linear Growth Model (Piecewise)

일정한 성장률을 가지는 경우 Linear Growth Model을 사용한다.\
Change point는 자동으로 탐지되며, 예측할 때 특정 지점이 Change point인지 여부를 확률적으로 결정한다. 그리고 Change point 는 신제품 출시, 디자인 변경과 같은 특정한 이벤트로 인해 추세가 변할 수 있는 시점이며, 사용자가 change point를 추가할 수도 있다.

![](https://media.vlpt.us/images/ubiradio/post/a2920aca-e6c2-432e-a54d-15c240e38025/linear\_growth.JPG)

### **Seasonality: s(t)**

시간에 따라 주기적으로 나타나는 패턴이다. 패턴은 Trend에 따라 진폭이 일정, 증가 혹은 감소할 수 있다. 푸리에 급수를 이용해서 패턴의 근사치를 찾는데, 푸리에 급수는 임의의 주기함수를 삼각함수의 합으로 표현할 수 있기 때문이다.

![](https://media.vlpt.us/images/ubiradio/post/d0fb6ed2-4f0b-45e8-86c5-e9d968702ade/prophet\_seasonality.jpg)

P는 주기 T를 의미하며 연단위면 365.25, 주 단위면 7일이다. \
N 은 삼각함수를 얼마나 섞는냐를 의미하며, 연도기준이면 N=10, 주단위면 N=3 이 잘 들어맞는다고 한다. N이 크면 패텬이 빠르게 바뀌게 되고, N이 작으면 느리게 변한다.

### **Holidays: h(t)**

주기성을 가지지는 않지만(불규칙하지만) 전체 추이에 큰 영향을 주는 이벤트이다. \
이벤트 앞뒤로 window 범위를 지정하여 해당 이벤트가 미치는 영향의 범위를 설정할 수 있다. 블랙프라이데이나 음력 이벤트는 기업의 생산관리나 이윤창출에 영향을 주지만 날짜가 다를 수 있다.

![](https://media.vlpt.us/images/ubiradio/post/c4be706d-b08c-440f-a6c9-67c64e9b7995/holiday.JPG)

**Error Term (ϵ)**\
데이터에 잡음이 들어가거나 유실되는 등 여러 가지 이유로 완벽한 회귀식을 만들 수 없기 때문에 회귀식에 정규분포를 따르는 잔차를 둔다.

### 모델 훈련

백엔드로 통계 모델링 도구 Stan을 사용해 모델을 학습시킨다. Prophet은 정확도가 높고 직관적인 파라미터를 조정해 모델을 튜닝할 수 있다.

모델을 적합하는 과정을 살펴보기 위, fit() 함수를 호출하면

```
y: history
seasonality_prior_scale: prior_scale
holidays_prior_scale: prior_scale
seasonality_mode: mode
period: period
```

등의\
변수와 파라미터를 입력받아 make\_all\_seasonality\_features() 함수를 호출하면 make\_seasonality\_features()에서 fourier\_series() 함수를 사용해 계절 패턴인 seasonal\_feature를 구하고 seasonality\_prior\_scale 파라미터를 사용해 prior\_scales 구하고 make\_holiday\_features() 함수를 사용해 holiday\_feature와 holidays\_prior\_scale 파라미터를 사용해 prior\_scales를 구해 seasonal\_features 에 추가한다.

```
seasonal_features: X,
prior_scales: sigmas,
changepoint_prior_scale: tau,
changepoints_t: t_change,
ds -> t: t
additive_terms: s_a
multiplicative_terms: s_m
```

등으로\
파라미터 및 변수를 정의하고 stan에 전달하여 모델을 학습한다. 모델 최적화 알고리즘은 뉴턴방법인 Newton 혹은 LBFGS를 사용한다.

```
transformed parameters {
   vector[T] trend;
   if (trend_indicator == 0) {
      trend = linear_trend(k, m, delta, t, A, t_change);
   } else if (trend_indicator == 1) {
      trend = logistic_trend(k, m, delta, t, cap, A, t_change, S);
   } else if (trend_indicator == 2) {
      trend = flat_trend(m, T);
   }
}

model {
   //priors
   k ~ normal(0, 5);
   m ~ normal(0, 5);
   delta ~ double_exponential(0, tau);
   sigma_obs ~ normal(0, 0.5);
   beta ~ normal(0, sigmas);

   // Likelihood
   y ~ normal(
   trend
   .* (1 + X * (beta .* s_m))
   + X * (beta .* s_a),
   sigma_obs
   );
}
```

Capacities\
시계열 데이터 전체의 최대값이다.\
예) 시장 총 수요

Change points\
추세가 변화하는 시점이다.\
예) 상품이 바뀌거나 신제품이 출시될 때

Holidays & Seasonality\
추세에 영향을 미치는 시기적 요인들이다.\
예) 판매량에 영향을 많이 미치는 휴일 등이다.

Smoothing Parameter\
각각의 요소들이 전체 추이에 미치는 영향의 정도이다.\
예) 주기마다 변동을 얼마나 나타내야 하는지이다.

### 모델 평가 <a href="#undefined" id="undefined"></a>

T 까지 자료가 있고 이후 h를 더 예측하는데 그 사이의 거리를 계산하고 오차의 MAPE(Mean Absolute Percentage Error)를 평가지표로 사용한다.

![](https://media.vlpt.us/images/ubiradio/post/4cf9b04a-7e40-40ab-9b23-17e55d785c4b/evaluation.JPG)

모델 평가 방법은 데이터가 시계열이고 섞을 수가 없기 때문에 대충 전체 기간의 절반 정도를 대상으로 윈도우를 잡고 평가한다. 윈도우가 크면 데이터가 다 비슷하고 작으면 에러가 커질 수 있다.

에러율 제일 낮아야 좋은 모델이다.

모델 튜닝 방법은 Baseline 모델과 비교해 성능이 떨어지면 trend, seansonality 등을 수정하고,\
특정 일자에 예측률이 떨어지면, 이상치를 제거하고,\
특정 cutoff (연말 등)에 예측률이 떨어지면 changepoint를 추가하는 방법으로 튜닝 작업을 할 수 있다.

### 참고자료

[https://github.com/facebook/prophet/blob/master/python/prophet/forecaster.py](https://github.com/facebook/prophet/blob/master/python/prophet/forecaster.py)\
[https://databricks.com/blog/2020/01/27/time-series-forecasting-prophet-spark.html](https://databricks.com/blog/2020/01/27/time-series-forecasting-prophet-spark.html)\
[https://databricks.com/blog/2021/04/06/fine-grained-time-series-forecasting-at-scale-with-](https://databricks.com/blog/2021/04/06/fine-grained-time-series-forecasting-at-scale-with-)\
[https://www.slideshare.net/lumiamitie/facebook-prophet](https://www.slideshare.net/lumiamitie/facebook-prophet)\
[https://brunch.co.kr/@gimmesilver/17](https://brunch.co.kr/@gimmesilver/17)\
[https://m-insideout.tistory.com/m/13](https://m-insideout.tistory.com/m/13)\
[https://peerj.com/preprints/3190.pdf](https://peerj.com/preprints/3190.pdf)\
[https://github.com/facebook/prophet/blob/master/python/prophet/forecaster.py#L1136](https://github.com/facebook/prophet/blob/master/python/prophet/forecaster.py#L1136)\
스파크를 이용한 시계열 예측, [https://towardsdatascience.com/pyspark-forecasting-with-pandas-udf-and-fb-prophet-e9d70f86d802](https://towardsdatascience.com/pyspark-forecasting-with-pandas-udf-and-fb-prophet-e9d70f86d802)
