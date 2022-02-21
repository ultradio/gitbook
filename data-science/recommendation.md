# Recommendation

### RNN을 이용한 아이템 사용 순서 추천

RNN을 이용한 아이템 추천은 사용자가 선택한 아이템을 시간순으로 정렬하여, Sequence Length (=2) 만큼 순서데로 데이터를 Representation 한 후, Sequence를 학습하고 예측모델을 생성한다. 생성한 예측 모델 기반으로 사용자가 아이템을 선택하면 다음에 사용할 아이템을 추천한다.

전처리할 때 사용자별 시간 순으로 아이템을 정렬하고, 연속된 중복 아이템 제거한다.

![](https://media.vlpt.us/images/ubiradio/post/9de28e6e-fd2b-4316-a309-79b91f55b701/RNN.jpg)

### ALS을 이용한 아이템 추천

Matrix Factorization 모델을 이용한 추천 알고리즘으로 잠재요인 모델 중 하나이다. 사용자/아이템 개수 대비 데이터가 드물게 존재하는 경우에도 적용이 가능하면 노이즈 데이터에 강하다.

![](https://media.vlpt.us/images/ubiradio/post/dcea6cc7-ff52-4ec7-9b1c-5429cb013c62/ALS.jpg)

사용자별로 아이템 사용 횟수에 대한 정보를 행렬로 구성하고 행렬 분해와 행렬 채우기를 통해 사용자별로 유사도 값을 계산한다. 특정 사용자와 가장 유사한 사람을 찾아 유사한 사람이 많이 사용한 아이템을 추천한다.

![](https://media.vlpt.us/images/ubiradio/post/9c9969dc-57b6-4cd1-aa9e-7aaf9a8c90b8/ALS2.jpg)

### 참고자료

[https://tykimos.github.io/lecture/](https://tykimos.github.io/lecture/)

