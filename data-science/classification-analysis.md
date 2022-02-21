---
description: Model Evaluation, Confusion Matrix, Accuracy, F1-Score, ROC-AUC
---

# Classification Analysis

### **분류 모형 모델링 과정**

* Class를 예측할 수 있는 Classification 모델을 선정한다.
* 데이터를 학습용과 검증용 데이터로 분리하고, Random Sampling하여 Class 비율을 조정한다.
* 분류 모델을 만들고 예측한다.
* 모델 성능 지표를 확인하고 모델의 정확도를 검증한다.

### Evaluation for Classification Model

모델평가는 평가지표를 사용하여 모델 성능을 평가하는 단계이다. 회귀모델이냐 분류모델이냐에 따라 평가지표와 방법이 다르다.

분류 모델 성능을 평가할 때는 얼마나 정확하게 데이터를 잘 분류했는지 평가해야 한다. 분류 결과를 담고 있는 혼동행렬(Confusion Matrix)을 이용하여 평가할 수 있다. \
일반적으로 정확도(Accuracy)를 이용하여 평가하지만, 데이터가 불균형이 심할 경우, F1-Score로 평가한다. \
이진분류에서는 ROC-AUC 평가 방법을 사용한다.

#### **Confusion Matrix**

혼동 행렬은 분류 모델의 성능을 평가하기 위한 행렬로 TP, FN, FP, TN 지표를 제공하는데, 이들의 조합으로 분류기 성능을 평가할 지표를 산출할 수 있다.

![](https://media.vlpt.us/images/ubiradio/post/e55b2d69-6d95-4bc8-8658-1df086d35604/confusionMatrxiUpdated.jpg)

예를 들어, 스팸 분류기 성능을 평가해보자.\
스팸분류기가 스팸메일로 예측한 경우를 긍정적인 것(Positive)으로, 일반메일로 예측한 경우를 부정적인 것(Negative)으로 정의한다.

정탐 (True)\
Positive든 Negative 든 정답을 잘 탐지한 것을 정탐이라고 한다. 분류기가 스팸을 스팸으로, 일반메일을 일반메일로 잘 분류한 경우이다.\
TP -> True (정답, 잘) Positive (긍정으로 예측)\
TN -> True (정답, 잘) Negative (부정으로 예측)

오탐 (type1 error)\
일반메일을 스팸으로 잘못 분류한 경우를 오탐이라고 하며, 기각해야할 가설을 채택하는 1종 오류이다.\
FP -> False (오답, 잘못) Positive (긍정으로 예측)

미탐 (type2 error)\
스팸을 일반메일로 잘못 분류한 경우를 미탐이라고 하며, 채택해야할 가설을 기각하는 2종 오류이다.\
FN -> False (오답, 잘못) Negative (부정으로 예측)

오탐과 미탐 중에 미탐에 더 주위를 기울여야 한다. **오탐은 원인 분석을 통해 탐지 가능**하지만, 미탐은 분석조차 할 수 없기 때문이다.

#### **Accuracy vs F1-Score**

TP, FN, FP, TN 지표로 이용하여 추가적으로 accuracy, precision, sensitivity, specificity 지표를 산출할 수 있다.

Accuracy\
정확도는 전체에서 모델이 정답으로 잘 분류한 것의 비율이다. Class 분포가 비슷할 때 사용한다.

Accuracy = (TP + TN) / (TP + TN + FP + FN)

#### Precision

정밀도는 **모델이 Positive로 예측한 것 중에** 실제로 정답이 Positive인 비율이다. \
모델이 Negative로 예측한 것 중에 실제로 정답이 Negative인 비율도 Precision이다.

Precision = TP / (TP + FP)\
Precision = TN / (TN + FN)

#### Recall, TPR (True Positive Rate, Sensitivity, 민감도)

재현율은 **실제 정답이 Positive인 것 중에** 모델이 Positive로 잘 예측한 비율이다. 실제 정답이 Negative인 것 중에 모델이 Negative로 잘 에측한 비율도 Recall이다.

Recall = TP / (TP + FN)\
Recall = TN / (TN + FP)

#### F1-Score

F1-Score는 Class 분포가 불균형이 심할 때 Accuracy 단점을 보완한 F1-Score를 사용한다.\
Precision과 Recall 의 조화평균으로 정밀도와 재현율이 어느 한쪽으로 치우치지 않을 때 1에 가까운 값을 가진다. \
F1-Score는 0\~1 사이 값을 가지며 1에 가까울 수록 좋다.

F1-Score = 2 x _Precision x_ Recall / (Precision + Recall)

#### ROC-AUC

ROC-AUC는 보통 이진분류에서 많이 사용하는 평가방법이다.\
민감도(TPR)와 특이도(TNR)를 이용하여 모델을 평가한다. ROC Curve는 분류 모델의 성능을 보여주는 그래프이고 ROC Curve 아래 면적을 AUC라고 한다. ROC Curve가 볼록한 형태이면서 AUC가 클수록 모델 성능도 좋은 것이다.\
AUC는 0\~1 사이의 값을 가지며 1에 가까울 수록 좋다. 보통 랜덤으로 선택하면 AUC가 0.5에 가깝고 ROC Curve는 직선에 가까워진다.

TNR (True Negative Rate, Specificity, 특이도)\
TNR은 실제 정답이 Negative 인 것 중에 모델이 Negative로 잘 예측한 비율이다.\
TN/(TN+FP)

FPR (False Positive Rate)\
FPR는 실제 정답이 Native인 것 중에 모델이 Positive로 잘못 예측한 비율이다.\
FPR = FP/(FP+TN)\
1- TNR

참고로 다중분류는 OvR(One-vs-Rest) 문제로 자기 클래스는 Positive 나머지는 모두 Negative로 하여 계산한다.

![](https://media.vlpt.us/images/ubiradio/post/636ad147-03cb-4ef8-9dde-2047d50a5703/multiple\_class\_confusion\_matrix.jpg)

### 참고자료

[https://medium.com/swlh/how-to-remember-all-these-classification-concepts-forever-761c065be33](https://medium.com/swlh/how-to-remember-all-these-classification-concepts-forever-761c065be33)

