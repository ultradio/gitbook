---
description: Correlation Analysis, Correlation Coefficient, Pearson, Spearman, Kendall
---

# Correlation Analysis

### Correlation Analysis

**두 변수간의 선형 관계가 있는지 파악**하기 위해 상관계수(Correlation Coefficient , r)를 구하여 상관분석을 할 수 있다.

상관계수 r은 -1과 1 사이의 값을 가지며, 상관계수가 0\<r≤+1 이면 양의 상관, -1≤r<0 이면 음의 상관, r=0이면 선형 관계가 없다 라고 한다.\
상관관계는 기울기가 아니고 직선의 정도를 나타내기 때문에, 기울기를 알 수는 없지만, X변수와 Y변수가 선형관계가 아니면 기울기는 0이고 선형관계이면 기울기가 0이 아니라는 것을 알 수는 있다.

**상관분석은 온도와 전력수요, 통화증가율과 물가상승률 등과 같은 두 변수 간의 선형 관계가 있는지 분석할 때 사용한다.**

상관분석할 때 유의할 점은 상관계수가 크다고 해서 상관성이 있다고 해석하면 안되며, 상관계수가 작다고 해서 두 변수간 관계가 없는 것도 아니다.\
상관계수의 절대값이 1에 가까워도 두 변수의 선형성이 뚜렷하지 않을 수 있다. 몇 개의 관측치로 인해 관계가 없는 변수도 상관계수가 높아질 수 있기 때문이다.\
또한, 상관계수는 선형성만 파악해 주기 때문에 곡선관계가 있는데도 상관계수가 0 일 수 있다.

따라서, 데이터에 Outlier가 존재하는지, 두 변수간의 비선형 관계가 있는지도 파악해봐야 한다.\
**산점도로 이용해 데이터 분포를 확인해서 만약, Outlier 가 존재하면 제거하고 비선형 관계가 있다면 선형으로 바꾸고 상관분석할 수 있다.**

![](https://media.vlpt.us/images/ubiradio/post/31ed61a3-0a1e-40fe-93d7-8985a70e4a32/correlation.jpg)

### **상관계수 종류**

#### Pearson Correlation Coefficient

피어슨 상관계수는 키, **체중과 같은 연속형 데이터의 상관계수를 측정**할 때 사용한다. 모수 검정이다.

#### **Spearman / Kendall Correlation Coefficient**

스피어만과 켄달 순위 상관계수는 **성적과 같은 순서형 데이터 상관계수를 측정**할 때 사용한다. 비모수 검정이다. 켄달은 데이터가 동률일 때 많이 유용하다.


**두 변수 간에 상관 관계가 있는지 확인할 때 상관계수를 구하고 상관분석을 했다면, 두 변수 간의 인과 관계를 파악할 때는 결정계수 구하고 회귀분석을 수행한다.**

### 참고자료

[https://bioinformaticsandme.tistory.com/58](https://bioinformaticsandme.tistory.com/58)

