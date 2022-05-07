# MLflow

End-to-End 머신러닝 라이프 사이클을 관리할 수 있는 오픈소스이다.

### MLflow 기능

#### 실험추적

하이퍼파라미터(learning\_rate, epoch 등)와 실혐 결과(accuracy 등) 를 기록하고 비교하여 실험을 추적할 수 있다.

#### 코드 패키징

데이터 사이언티스트와 ML 코드를 공유하거나, 프로덕션으로 배포하기 위해 재사용 가능한 형식으로 ML코드를 패키징 할 수 있다.

#### 모델

다양한 머신러닝 프레임워크의 모델을 관리하고 배포할 수 있다.

소규 프로젝트에서 실험용으로는 적합하지만 프로덕션 환경에 적용하기는 많이 부족하다.

![출처: https://databricks.com/wp-content/uploads/2020/06/blog-mlflow-model-1.png](<../.gitbook/assets/spark/spark_mlflow.png>)

### MLflow와 Kubeflow의 차이점

#### MLflow

개인 또는 팀간 로컬 환경에서 손쉬운 협업이 가능하며, 소규모 프로젝트에서 실험용으로는 적합하지만\
프로덕션 환경에 적용하기는 많이 부족하다.

#### Kubeflow

개인 또는 팀간 협업을 위해 Kubeflow와 Kubernetes에 대한 사전 지식이 필요하다.\
ML 파이프라인 자동화를 제공한다.

### 참고자료

Azure Databricks Documentation, https://docs.microsoft.com/ko-kr/azure/databricks/applications/mlflow/ \
MLflow vs Kubeflows. https://docs.microsoft.com/ko-kr/azure/databricks/applications/mlflow/
