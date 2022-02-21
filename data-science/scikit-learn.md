---
description: scikit-learn
---

# scikit-learn

### Data Preparation <a href="#data-preperation" id="data-preperation"></a>

#### 라이브러리 가져오기

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

#### 데이터세트 로드

```python
data = pd.read_csv('/kaggle/input/ecommerce/advertising.csv')
data.head(2)
```

### Data Exploratory Analysis <a href="#data-exploratory-analysis" id="data-exploratory-analysis"></a>

#### 데이터 탐색 및 전처리

```python
data.info()
```

Data Type, Data Count, Missing Value 를 확인한다.\
Data Type을 점검하여 Type 변경이 필요한지,\
Data Count를 점검하여 Train과 Test 는 어떤 비율로 나눌지,\
Missing Value를 점검하여 제거할지 대체할지를 정한다.

#### 기초통계분석(Descriptive Statistics Analysis)

수치형 데이터

```python
data.describe()
```

mean, median, min, max 등 기초통계로 분포와 Outlier를 예상해 본다.\
데이터가 평균근처에 있고 정규분포로 예상되면 큰 신경쓰지 않아도 된다.\
: 25% - min, max - 75% 를 차이를 확인하면 분포의 치우침 정도를 예상해 볼 수 있다.\
: 25% - min 차이가 크면 오른쪽으로 치우친 분포이고 max - 75가 크면 왼쪽으로 치우친 분포일 가능성이 크다.\
min이 상대적으로 너무 작거나 max가 너무 크면 Outlier가 존재할 것으로 추측해보고 차트를 그려본다.

차트로 분포를 점검

```python
sns.distplot(data[['Age']])
```

범주형 데이터

```python
data['Country'].nunique()
data['Country'].unique()
```

Outlier 를 점검한다.

```python
plt.figure(figsize=(20,10))
sns.boxplot(x='productline', y='startprice', data=data)
```

Q3 + IRQ_1.5 과 Q1 - IRQ_1.5 밖은 outlier로 본다.

범주형을 수치형으로 변경한다.

```python
pd.get_dummies(data, columns=['zip_code', 'channel'], drop_first=True)
```

#### Missing Value

```python
data.isna().mean()
```

전체 데이터 건수에서 Missing Value가 차지하는 비율을 점검해본다.

```python
df = data.dropna()
df.isna().mean()
```

Missing Value를 삭제할 수도 있지만, 제거하면 중요한 정보도 함께 제거될 수 있으므로 되도록이면 mean, median 등으로 대체한다. 예를들어, Missing Value가 20% 정도면 impute 하고, 40%\~50% 정도면 feature가 많지 않을 경우에는 제거하는 것보다 impute 하는 것이 좋다. 보통, Missing Value는 Impute는 mean, median을 많이 사용한다.

```python
data.fillna(round(data['Age'].mean()), inplace=True)
data.isna().sum()
```

### Data Representation <a href="#data-representation" id="data-representation"></a>

```python
from sklearn.model_selection import train_test_split
X = data[['X1', 'X2', 'X3']]
y = data['Y'] #Series
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 100)
```

보통, 전체 데이터가 1000 개 이하로 적으면 훈련용과 검증용 데이터를 8:2 나누고, 그 이상이면 7:3로 나눈다.

### Data Modeling <a href="#data-modeling" id="data-modeling"></a>

```python
Logistic Regression
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)

model.coef_
```

절대값이 클수록 영향도가 높은 변수지만, 데이터의 스케일 다르면 꼭 그렇지도 않으므로니 주의해야 한다.

#### Decision Tree

gini 지수가 0.5이면, 불순물이 가장 많은 것이고 0에 가까울 수록 불순물 없이 잘 분류한 것이다.

```python
from sklearn.tree import plot_tree
```

#### DecisionTreeClassifier

```python
acc_list = []
for i in range(2, 31):
    model = DecisionTreeClassifier(max_depth = i)
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    print(i, round(accuracy_score(y_test, pred), 4))
    acc_list.append(accuracy_score(y_test, pred))  
```

```python
from sklearn.ensemble import RandomForestClassifier
```

#### RandomForestRegressor

```python
rf = RandomForestRegressor(n_estimators=150, max_depth = 10, random_state = 100)
rf.fit(X_train, y_train)
pred = rf.predict(X_test)
rf.feature_importances_
```

#### KMeans

```python
from sklearn.cluster import KMeans
model = KMeans(n_clusters=3)
model.fit(data)
```

### Model Evaluation & Optimization <a href="#model-evaluation--optimization" id="model-evaluation--optimization"></a>

```python
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

pred = model.predict(X_test)
accuracy_score(y_test, pred)
confusion_matrix(y_test, pred)
```

Accuracy는 데이터 불균형이 심하면 모델 성능을 정확히 파악하기 어렵기 때문에, Confusion Matrix를 확인해보고 잘못 예측한 비율(type1/type2 error)이 많다면 Precision, Recall, f1-score를 확인해서 최종 모델 성능을 점검해봐야 한다.

비지니스 문제에 따라 type 1/type 2 error의 중요도 차이가 있다.\
예를 들어, **실제 암이 아니지만, 암으로 예측할 경우(type 1 error)는 큰 문제가 없지만, 암인데 정상으로 예측(type 2 error)하면 심각할 수 있다.**\
**type 1 Error가 중요한 경우는 마케팅 비용 지출에 따른 구매 반응을 분석할 때 type 1 error는 마케팅 비용을 지출했지만 실제로 구매가 이루어지지 않았으므로 마케팅 비용 지출이 발생한다.**

Confusion Matrix

| 실제값\예측값 | 0           | 1          |
| ------- | ----------- | ---------- |
| 0       | TN          | FP (Type1) |
| 1       | FN (Type 2) | TP         |

Clustering 평가

클러스터링한 label을 데이터세트와 합쳐서 클러스터별 변수들의 평균을 구하고 분석에 활용한다. 예를들어, 나이, 성별, 연봉, 소비점수가 주어졌을 때 고객 분류하기 위해 클러스터하여 나이가 어리고 수입이 높으면 지출이 높다. 나이가 많고 수입이 높은 그룹은 적은 그룹보다 지출이 적다. 등의 분석을 할 수 있다.

Clustering 최적화

```python
distance = []
for i in range(2,11):
    model = KMeans(n_clusters=i)
    model.fit(df)
    distance.append(model.inertia_)
```

Elbow는 최적의 클러스터 개수를 선정하기 애매할 때가 있으면 이럴 경우, 실루엣 점수를 사용해야 한다. inertia\_ 는 클러스터 중심과 관측치와의 거리 합으로 작을 수록 좋지만, 실루엣 스코어 값이 클수록 좋다.

```python
sil = []
for i in range(2,11):
    model = KMeans(n_clusters=i)
    model.fit(data)
    sil.append(silhouette_score(data, model.labels_))

시각화
plt.figure(figsize=(20, 10))
```

Clustering 최적화 시각화

```python
from sklearn.decomposition import PCA
pca = PCA(n_components=2)
pca.fit(data)
pca_df = pca.transform(data)
pca_df = pd.DataFrame(pca_df, columns=['PC1', 'PC2'])
pca_df.head(2)
```

차원축소하면 데이터가 왜곡되어 클러스터링이 잘 안된 것 처럼 보일 수 있지만, 감안하고 시각화 분석을 수행해야 한다.

### Logistic Regression, Decision Tree, Random Forest 의 차이

Logistic Regression은 Parameter이고, Decision Tree는 Non Parameter이다.\
Logistic Regression는 Feature Power를 좀더 잘 드러낸다.\
Decision Tree는 Categorical Value를 수치형으로 바꾸지 않아도 된다.\
Tree 계열은 LR처럼 변수의 영향도를 명확하게 파악할 수는 없지만 상대적으로 어떤 변수가 중요한지 파악할 수는 있다.

Bagging 과 Random Forest는 Decision Tree의 overfitting 문제를 해결하기 위해 나온 알고리즘이다.\
Bagging은 전체 데이터를 여러 개의 서브세트로 분리하고 각각 Tree를 학습하고 그 결과를 평균내어 예측을 하는데, 특정 feature에 영향도 높을 경우 모델 성능이 좋지 않을 수도 있다.\
Random Forest는 Bagging에서 좀 더 발전한 알고리즘으로 **Bagging 관측치를 샘플링해서 서브세트를 만들었다면, Random Forest는 독립변수를 샘플해서 서로 다른 데이터세트를 만들고 그 결과를 평균내어 예측한다.** 영향도가 큰 변수의 영향도를 낮추어 다른 변수의 특성도 파악할 수 있는 장점이 있다.

### 활용사례 <a href="#undefined" id="undefined"></a>

광고 반응률 예측하기\
\- 나이, 성별, 수입, 일평균 인터넷 사용시간이 주어졌을 때 광고 클릭 반응 분석하기\
구매 요인 분석하기\
\- 통신사, 제품색상, 가격이 주어졌을 때 제품 구매 요인 파악해 보기\
프로모션에 반응할 고객 예측\
\- 최근방문일, 채널, 반품 독립변수가 주어졌을 때 구매여부를 예측하기\
고객분류\
\- 나이, 성별, 연봉, 소비점수가 주어졌을 때 고객 분류
