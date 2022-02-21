---
description: Katib, Katib Component, Katib Admission Webhooks, Katib Workflow
---

# Kubeflow Katib

### Katib

Katib은 AutoML 도구 중 하나이다. ML에서 모델링할 때 상당 부분이 반복 실험을 통해 Hyperparameter를 찾는 과정인데, 이 과정을 자동화 한 것이 AutoML이다.\
AutoML은 Auto Feature Engineering, Hyper Parameter Optimization, Neural Architecture Search를 제공한다. Katib 버전 0.11에서는 Hyper Parameter Optimization, Neural Architecture Search 를 지원한다.

#### HyperOpt (Hyperparameter Optimization)

Hyperparameter는 Learning rate, Dropout rate, Cost function 과 같이 실험할 때 사용자가 정의하는 값이다. 최고의 정확도를 내는 ML 모델을 만들기 위해 이러한 **Hyperparameter 찾는 일련의 작업을 Hyperparameter Optimization**라고 한다.

#### NAS (Neural Architecture Search)

NAS는 최적의 Neural Network를 디자인하기 위해 사용하며, Katib에 제공하는 NAS는 강화학습을 사용하여 Network를 디자인한다.

### Katib Component

Katib는 크게 Experiment, Suggestion, Trial, Worker Job 컴포넌트로 이루어져 있다.

![](https://cdn-images-1.medium.com/max/1200/1\*\_hJ0F4v4WXchERjJuC3LMQ.png)

TFJob을 예제로 컴포넌트 간에 관계를 살펴보면, 병렬로 실행할 Trial 수를 3개, 각 Trial 마다 Worker 2개를 설정했다면, 다음과 같이 병렬로 튜닝 작업을 진행한다.

![](https://cdn-images-1.medium.com/max/1200/1\*osjz6F7kbbTnQ5RzmqLdgw.png)

#### Experiment

Experiment는 목표 값을 만족시키는 Hyperparameter를 찾는 일련의 탐색 작업으로 **Experiment CR을 작성하여 Tuning 작업을 정의**할 수 있다.

#### Suggestion

Experiment 마다 Suggestion CR 하나를 생성하는데, Experiment Controller에서 Experiment CR의 Search Parameter와 Search Algorithm 설정에 따라 내부적으로 Suggestion CR을 생성하고 Suggestion Controller에 전달하면, **Suggestion Controller는 Search Algorithm을 생성한 후, Search Algorithm에서 제안한 Hyperparameter를 Experiment Controller에 전달**한다.

#### Trial

Experiment Controller에서 Suggestion Controller로 부터 전달 받은 Hyperparameter 세트 마다 Trial CR을 생성한 후, Trial Controller에 전달하면, Trial Controller는 Experiment의 Trial 수 만큼 Trial을 생성하고, Trial 마다 **Suggestion에서 추천 받은 Hyperparameter로 Worker Job을 생성**한다.

#### Worker Job

실제로 학습을 수행하는 일을 하며, MPIJob, TFJob, PyTorchJob, Kubernetes Job 을 실행할 수 있다.

#### Metrics Collector

Metrics Collector는 Worker Job(MPIJob, TFJob, PyTorchJob)에 Sidecar로 Injection하여 Objective 설정에서 정의한 목표 값을 찾기 위해 Metrics를 수집하고 Katib DB에 저장한다.

#### Katib Admission Webhooks

Kaitb는 Experiment CR 생성을 요청하면, admission webhook에서 요청한 CR을 변형(Mutate)하고 검증(Validate)해서 요청을 수용할지 판단한다.

### Katib Workflow

![](https://cdn-images-1.medium.com/max/1200/1\*NXasGja1sgntCQK8ZYWQNg.png)

### 참고자료

[https://www.kubeflow.org/docs/components/katib/](https://www.kubeflow.org/docs/components/katib/)\
[https://github.com/kubeflow/katib/blob/master/docs/developer-guide.md#katib-admission-webhooks](https://github.com/kubeflow/katib/blob/master/docs/developer-guide.md#katib-admission-webhooks)\
[https://arxiv.org/pdf/2006.02085.pdf](https://arxiv.org/pdf/2006.02085.pdf)\
