---
description: ML Workflow, Kubeflow, Kubeflow Component, Multi-Tenancy
---

# Kubeflow

### ML Workflow?

데이터 문제 해결하기 위해 ML을 이용한 데이터 분석 과정을 살펴보면, 데이터 전처리, 탐색적 데이터 분석, 데이터 표현, 모델링, 모델 최적화 등의 과정을 거쳐 ML모델을 생성한다. 생성한 ML모델을 Serving시스템에 deploy한 후, 새로운 데이터를 예측하는 Inference Service를 만들 수 있다. 이러한 일련의 과정을 ML Workflow라고 한다.

#### **ML Workflow가 왜 필요한가?**

ML에 필요한 GPU, Memory 등 물리적인 리소스와 ML Framework가 발전하면서 ML모델을 학습시키기 위해 들어가는 시간과 노력이 점점 줄어들고 있다. 하지만, 데이터 분석 과정에서 많은 작업들을 수동으로 진행해야 한다. **ML Workflow는 데이터 분석 Workflow를 자동화해주는 도구**이다.

#### **ML Workflow를 어떻게 도입해야 하나?**

대표적인 ML Workflow 도구로 Public Cloud에서 제공하는 **Google GCP, AWS SageMaker, Azure ML**이 있으며, 오픈소스로는 **Kubeflow**가 있다. 비용이 여유가 있고 서비스 해야할 모델이 Public Cloud에서 제공해주는 경우, Public Cloud를 사용해서 구축하여 운영에 소비되는 자원을 줄이는 것이 맞다. 하지만, 그렇지 않는 경우에는 자체 개발이나 오픈소스를 활용하여 ML Workflow를 구성해야 할 수도 있다.

#### **오픈소스로 ML Workflow를 구성할 때 고려할 사항은 무엇인가?**

**하나의 플랫폼** 안에서 모든 작업이 가능한가?\
오픈소스 버전이 **프로덕션 레벨**에서 사용할 정도로 안정적인가?\
오픈소스가 **다양한 ML프레임워크**를 지원하는가?\
하이퍼 파라미터 최적화 라이브러리를 제공하는가?\
**ML 모델 버전관리**를 제공하는가?\
각 ML Workflow 단계를 수행할 수 있는 SDK 및 라이브러리가 있는가?\
리소스 인증/권한 관리를 제공하는가?\
멀티 태넌시를 지원하는가?\
병렬 처리를 지원하는가?\
**타 인프라와의 연동**이 원활한가?

### Kubeflow

오픈소스 ML Workflow Kubeflow에 대해 알아보자.\
\
Kubeflow가 추구하는 목표는 ML Workflow에 필요한 서비스를 직접 만드는 것이 아니라, 각 영역에서 가장 적합한 오픈소스를 제공하는 것이다.  Kubeflow는 Kubernetes 위에서 실행되는 ML 툴킷으로 어떤 새로운 서비스가 아닌 기존에 있던 오픈소스들의 묶은 것이다.

![](<../.gitbook/assets/image (4).png>)

### Kubeflow Component

Kubeflow의 Application은 High-Level Application과 Low-Level Application으로 나눌 수 있다. High-Level Application은 웹으로 노출한된 사용자 Application이, Low-Level Application을 이용해서 Workload를 실행한다.

![](https://miro.medium.com/max/960/0\*L1mkP8aUgxcRyB9r.png)

Kubeflow에서 제공하는 각 Component에 대해 간단히 알아보자.

#### Central Dashboard

대쉬보드 UI로 각 컴포넌트를 접근할 수 있다.

#### Notebook Servers

Kubernetes 상에서 실행되는 Jupyter Notebook Server로, 설정만으로도 간단히 Notebook을 생성할 수 있다.

#### Training Operators

TFJob, PyTorchJob, MPJob 을 활용해 분산학습으로 빠르게 ML모델을 만들 수 있다.

#### Katib

Katib은 AutoML 도구 중 하나이다. ML에서 모델링할 때 상당 부분이 반복 실험을 통해 하이퍼 파라미터를 찾는 과정이다. 이 과정을 자동화 한 것을 AutoML이라 한다.

#### KFServing

ML 프레임워크의 Serving 모델을 Kubeflow에서 사용할 있게 해주는 Serving 도구이다.

#### Kubeflow Pipelines

ML Workflow를 만들고 배포할 수 있다.

![](<../.gitbook/assets/image (5).png>)

### Multi-Tenancy

Kubernetes는 Namespace를 사용하여 Resource를 격리하는데, **Kubeflow는 Namespace 래퍼인 Profile CRD를 사용하여해 사용자 별 Resource 사용량을 제한**할 수 있다.

![](https://miro.medium.com/max/959/0\*zFlEXWjS6WSsnq7n.png)

### 참고자료

[https://www.kubeflow.org/docs/](https://www.kubeflow.org/docs/)

