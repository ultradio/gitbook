---
description: Istio
---

# Istio

마이크로서비스는 **서비스간의 연결 구조가 복잡해서 서비스들이 서로 직접 호출하면 장애가 발생할 때 추적이 어렵고, 로드밸런서, 트래픽 관리, 인증, 권한 부여 등을 관리하는 것이 복잡**해진다. 이러한 마이크로 서비스의 문제를 해결하기 위해 직접 호출하는 것이 아니라, 서비스 마다 프록시를 두고 프록시를 통해 서비스를 호출하여 트랙픽을 통제해야 한다.

보통 프록시는 서비스 내부가 아니라 서비스 컨테이너와 분리된 사이드카(Sidecar) 형태로 실행되는데, **서비스가 증가함에 따라 프록시도 증가하기 때문에, 프록시 관리가 어려워진다**. 그래서 프록시 관리 어려움을 줄이기 위해 프록시 설정값을 저장하는 Control Plane과 설정값에 따라 트랙픽을 제어하는 Data Plane으로 나눠서 프록시를 관리할 필요가 있다. 이러한 Service Mesh\* 구조로 네트워크 트래픽 관리하는 오픈소스 도구 중에 Istio가 있다.

## Istio

Ingress는 쿠버네티스 클러스터로 들어오는 트래픽을 관리하고, 이스티오는 **쿠버테니스 클러스터 내에 Service Mesh로 들어오고 나가는 트래픽을 관리**하기 위한 Gateway를 제공하여, 트래픽 라우팅, 로드밸런싱, 서비스-서비스 간 인증, 모니터링 기능을 수행한다.

이스티오는 Data Plane과 Control Plane 2개의 영역으로 구분되며, Data Plane으로 Envoy Proxy를 사용하고 Control Plane은 istiod가 담당한다.

![출처: https://istio.io/latest/docs/ops/deployment/architecture/](https://cdn-images-1.medium.com/max/1200/0\*t3WDlyYgxTPoNjF3.png)

### **Control Plane**

Control Plane은 **서비스 메쉬 컨트롤러**로 Pilot, Mixer, Citadel, Galley 로 이루어져 있다.

**Pilot**\
Envoy Proxy 설정관리로 서비스간 통신 및 트래픽 관리를 위해 Policy를 구성한다.

**Mixer**\
각종 모니터링 지표를 수집하며, Policy를 체크하는 역할을 한다.

**Citadel**\
서비스 간의 인증 관리 TLS\* 암호화, 인증서 관리를 한다.

**Galley**\
Istio Configuration 유효성 검사를 한다.

### **Data Plane**

Data Plane은 Envoy Proxy를 사용해 **서비스 메쉬를 구성**한다.

**Envoy Proxy**\
서비스간 통신 및 트래픽을 제어, URL 기반 라우팅(L7), 로드 밸런싱, Dynamic Configuration 기능을 제공한다.

### 이스티오 트래픽 관리 정책 (Istio Traffic Management)

**이스티오는 Gateway, VirtualService, DestinationRule, Service Entry 리소스를 사용하여 트래픽 관리 정책을 정의**한다.

**Gateway**

**Ingress Gatway에 적용되며,** 서비스 메쉬로 들어오는 외부 트래픽 허용 정책(프로토콜, 호스트, 포트)을 설정한다. 기본적으로 이스티오는 모든 외부 트래픽을 차단한다.

**VirtualService**

**Sidecar Proxy에 적용되며** Gateway정책과 맵핑하여 설정한다.\
**트래픽을 어디로 보낼지 라우팅 정책 설정**하며, 들어온 트래픽을 라우팅 규칙에 따라 적절한 서비스로 라우팅 하는 역할을 한다. Kubernetes의 Service가 목적지가 되며, 실제로 Virtual Service의 기능은 **URI나 Header 기반으로 라우팅하는 Ingress와 유사하다.**\
\
가중치를 비율을 적용하여 트래픽을 제어하는 기능도 있어 A/B 테스트에 활용할 수 있다.\
**참고로, Ingress는 가중치 비율로 트랙픽을 제어할 수 없다.**

**DestinationRule**

**Sidcar Proxy에 적용되며**, VirtualService 정책과 맵핑하여 설정한다.\
VirtualService가 라우팅해서 Service로 트래픽을 보내면, 실제로는 그 안에서 있는 Pod에 라우팅이 되는데, **어느 Pod에 어떻게 트래픽을 보낼지 정의한다. 서비스에 트래픽을 보내더라도 Pod에 기술된 Label에 따라 서비스 내의 Pod를 골라서 트래픽을 보내거나, Pod간의 로드밸런싱 등 라우팅 정책 설정이 가능하다.**

**ServiceEntry**

Egress Gateway에 적용되며, 외부로 나가는 트래픽 허용 정책 설정을 한다. 기본적으로 이스티오는 모든 외부 트래픽을 차단한다.

![출처: https://medium.com/google-cloud/back-to-microservices-with-istio-p1-827c872daa53](<../.gitbook/assets/image (2).png>)

Istio가 Ingress 대비 장점은 백분율 기반 라우팅, 트래픽 속도 제한 등이 있으며, 메트릭 수집도 매우 쉽다.

### Nginx Ingress Controller

Ingress는 쿠버네티스 클러스터로 들어오는 트래픽을 관리하며, Front에 Load Balancer 연결할 수 있다./etc/nginx/nginx.conf 파일에 라우팅 정보를 설정한다.

### 이스티오 기반 A/B 테스트 예제

![](https://lh3.googleusercontent.com/-rjU-XeJrx1w/YUL-6XtKb-I/AAAAAAAAaAM/bR0UQ4Q\_-Skz09S6IH2GGg5-lbULDYAlgCLcBGAsYHQ/w640-h306/image.png)

### 용어

Service Mesh\
마이크로 서비스 통신계층으로 서비스간 모든 통신이 메쉬를 통해 이루어진다.

SSL(Secure Sockets Layer)\
데이터를 암호화하여 안전하게 전송하기 위한 인터넷 통신 프로토콜

TLS(Transport Layer Security)\
SSL의 차세대 버전으로 모든 종류의 인터넷 트래픽을 암호화

### 참고자료

[https://istio.io/latest/docs/ops/deployment/architecture/](https://istio.io/latest/docs/ops/deployment/architecture/)\
[https://medium.com/google-cloud/back-to-microservices-with-istio-p1-827c872daa53](https://medium.com/google-cloud/back-to-microservices-with-istio-p1-827c872daa53)\
[https://github.com/istio/istio/tree/master/samples](https://github.com/istio/istio/tree/master/samples)\
[https://www.mirantis.com/blog/your-app-deserves-more-than-kubernetes-ingress-kubernetes-ingress-vs-istio-gateway-webinar/](https://www.mirantis.com/blog/your-app-deserves-more-than-kubernetes-ingress-kubernetes-ingress-vs-istio-gateway-webinar/)
