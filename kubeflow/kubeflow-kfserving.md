---
description: KFServing, Transformer, Predictor, Explainer
---

# KFServing (KServe)

본 문서는 KServe 0.8 버전 기준으로 작성하였다.

KServe는 **Kubernetes에 ML Model을 Deploy하고 Serving 기능을 제공하는 Model Inference Platform**이다.

### Control Plane
InferenceService CR 작성하고 Kubernetes API Server에 등록하면, Transformer, Predictor, Explainer 등을 생성하여 Inference Service 를 구축할 수 있다. Knative Serverless를 기반으로 트래픽이 없을 때는 scale-to-zero 동작한다.


Inference할 데이터셋 위치를 정의한다. Predictor에 ML Framework spec을 정의한 후 endpoint spec을 생성한다. 생성한 endpoint spec을 InferenceService Metadata spec에 작성해 InferenceService를 생성한다.

![출처: https://kserve.github.io/website/0.8/modelserving/control_plane/](<../.gitbook/assets/kserve_controlplane.jpg>)

#### Control Plane Components


### Data Plane

KF Serving 구성요소는 Endpoint, Transformer, Predictor, Explainer 가 있으며 Endpoint 마다 Explainer 를 구성하고 필요에 따라 Transformer, Explainer 를 추가할 수 있다.

#### Endpoint

InferenceService는 Default Endpoint와 Canary 2개를 제공하며, Rollout 정책을 정의하여 트래픽 비율을 조절할 수 있다.

#### Transformer

사용자가 Predictor나 Explainer 수행 전 후에 데이터 전처리, 후처리 할 수 있다.

#### Predictor

ML Model Server로 데이터를 예측하거나 분류하는 역할을 한다.

#### Explainer

XAI로 데이터를 **예측하거나 분류한 결과에 대해 판단 이유를 제시**하는 역할을 한다.

![출처: https://kserve.github.io/website/modelserving/data\_plane](https://cdn-images-1.medium.com/max/1200/0\*8\_QNAq0wxhDYrbNd.png)

### API v1

| API       | Method | Path                | Payload                                                                        |
| --------- | ------ | ------------------- | ------------------------------------------------------------------------------ |
| Readiness | GET    | /v1/models/         | Response:{"name": , "ready": true/false}                                       |
| Predict   | POST   | /v1/models/:predict | Request:{"instances": \[]} Response:{"predictions": \[]}                       |
| Explain   | POST   | /v1/models/:explain | Request:{"instances": \[]} Response:{"predictions": \[], "explainations": \[]} |


### 참고자료

[https://kserve.github.io/website/](https://kserve.github.io/website/)\
[https://www.kubeflow.org/docs/components/kfserving/](https://www.kubeflow.org/docs/components/kfserving/)\
[https://devocean.sk.com/blog/techBoardDetail.do?ID=163739](https://devocean.sk.com/blog/techBoardDetail.do?ID=163739)

> KFServing 에서 제공하는 Endpoint, Transformer, Explainer, Predictor 외에 더 구성요소를 추가할 예정이며, Outlier Detection 도 그 중 하나이다.
