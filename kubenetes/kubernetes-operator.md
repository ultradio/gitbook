---
description: CRD, Kubernetes Operator
---

# Kubernetes Operator

컨테이너 라이프사이클을 관리하기 위해 Kubernetes가 도입되었지만, 사람이 직접 유지보수해야 하는 부분이 존재한다. 사람이 반복적으로 수작업해야 하는 문제를 해결하기 위해 나온 것이 Kubernetes Operator이다.

Kubernetes Operator를 이해하려면, 먼저 CRD와 Custom Controller를 알아야 한다.

### CRD (Custom Resource Definition)

Kubernetes에서 애플리케이션을 개발하다 보면 어쩔수 없이 추가적인 기능이 필요할 수도 있고, Deployment, Service 등을 추상화하기 위한 **별도의 오브젝트가 필요할 때가 있다**. 이 때 Kubernetes에서 제공하는 CR(Custom Resource)를 사용하면 된다.\
CR은 kubernetes에서 내장하고 있는 Resource들 이외에 **사용자가 필요에 따라 새로 정의한 Resource**를 말하며, kubectl 같은 kubernetes 기본 명령어 사용도 가능하다.

Kubernetes Resource를 확장하기 위해 CR을 사용하려면, **CR에 어떤 항목이 정의되어야 하는지를 선언한 메타데이터 오브젝트인 CRD**를 먼저 생성하고 Kubernetes Api Server에 등록해야 한다.

CRD는 apiextensions.k8s.io/v1 확장해서 OpenAPI Schema v3(OAS 3.0) 스펙 기반으로 작성하기 때문에, OAS 3.0 기반으로 조건에 위배되는 항목이 있는지 Validation할 수 있다.\
※ 참고로, swagger는 OAS기반으로 REST API 설계, 빌드, 문서화에 사용하는 오픈소스 도구이다.

CRD를 작성한 후, Custom Controller를 구현하면, kubernetes API를 확장한 Custom Resource를 사용할 수 있다.

### Kubernetes Operator

Kubernetes Operator는 CRD와 Controller로 구성되는데,\
Custom Resource가 어떻게 동작해야 하고,\
어떻게 배포되어야 하고,\
(문제에 대해서) 어떻게 반응해야 하는지 등에 대해 정의하여 제어할 수 있다.

Kubernetes Operator의 동작 방식은

1\) CRD를 참고하여 원하는 상태를 CR에 작성하고 배포하면, (CR은 구조화된 데이터이 뿐)\
2\) Custom Controller가 CR을 모니터링하고 있다가 변경된 CR의 상태를 맞추기 위해 설치, 업그레이드 등 필요한 작업을 한다.

![](https://cdn-images-1.medium.com/max/1200/0\*P1sNiIEmKaLnvtbL.jpg)

#### Kubernetes API로 CR 등록

다음은 Kubernetes API로 CR을 동록할 때, Client와 Custom Controller 간의 동작 절차이다.

1\) Reflector는 Kubernetes API에서 지정한 Resource(kind)를 김시하다가,\
2\) 새로운 Resource 인스턴스(Object)가 감지되면 Delta Fifo 큐에 넣는다.\
3\) Informer가 Delta Fifo 큐에서 Object를 꺼내서,\
4\) 나중에 검색할 수 있도록 Indexer에 전달하면,\
5\) Indexer가 Object의 Key를 생성하고 해당 Key로 Object를 저장한다.\
6\) 그 다음, Informer는 Custom Controller 콜백함수로 Object의 Key를 전달하고\
7\) Workqueue에 넣으면\
8\) Key를 꺼내서\
9\) Index를 통해 Key로 Object를 가져온다.

![](https://cdn-images-1.medium.com/max/1200/0\*NL6wql3wjkqdvngv.jpeg)
