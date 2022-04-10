---
description: >-
  Kubernetes, Kubernetes Architecture, Object, Controller, Label, Selector,
  Annotation
---

# Kubernetes

### 쿠버네티스

쿠버네티스는 분산환경에서 컨테이너 라이프사이클을 관리하기 위한 도구이다.&#x20;

마이크로 서비스는 애플리케이션이 여러 개의 애플리케이션 컨테이너로 나누어져서 관리 포인트가 늘어났고 이를 관리해 줄 쿠버네티스가 필요하다. 쿠버네터스가 많이 사용되는 곳은 마이크로서비스, ML 워크로드 등이다.

#### 쿠버네티스 철학&#x20;

쿠버네티스는 시스템 구축을 위한 수작업을 최소하고, 시스템을 셀프 서비스로 운영하는 목적을 가지고 있다.

#### Master Node 구조&#x20;

쿠버네티스는 Master와 Note로 구성된다. Master는 쿠버네티스 클러스터 관리하는 역할을 하고 Node는 쿠버네티스 클러스터 내 워커로 VM 혹은 물리머신이다.

![](https://lh4.googleusercontent.com/N7V4xSo37cs6goHXXEZKBhE9d91SdfFX0-46Zif6vaEZgFio3uM5i\_xtAw0Fx-tOqgBJssbVIWspGFyK6d38THnSRR8FLJUEOKiHTqnrgXcMJ9W73VvgZd4UcvAJBzk\_meI9f1Iq=w640-h360)

### 오브젝트와 컨트롤러

#### **Object (오브젝트)**

#### Pod

파드는 쿠버네티스에서 배포할 수 있는 가장 작은 단위로 컨테이너 그룹의 **컨테이너 사이에 스토리지, IP 등 자원을 공유**한다. 컨테이너 런타임으로 도커를 주로 사용하며, 파드 라이플 사이클은 kubelet이 관리한다.

#### Volume

**파드가 사용하는 스토리지다.** 파드는 stateless 하므로 데이터 보존이 필요한 경우 볼륨을 만들고 파드에 붙여서 사용해야 한다. \
볼륨을 PV (Persistent Volume)라고 하며, PV 만드는 방법에는 미리 만들어 놓는 static 방법과 요청이 있을 때 만드는 dynamic 방법이 있다. dynamic 방법은 PVC에 storageClassName 항목에 StorageClass 명을 넣으면 StorageClass를 통해 PV를 생성한다. **PV는 직접 컨테이너와 연결되지 않으며, PVC에 정의한 조건에 맞는 PV가 있으면 바인딩한다.** 파는 PVC를 볼륨으로 인식하여 사용한다.

#### Service

파드에 접근할 수 있는 네트워크를 관리한다. 파드가 IP를 가지고 있어도 서비스가 없으면 클러스터 외부로 노출할 수 없다.  하지만 **SSL 인증서 처리나 URI 기반 라우팅이 불가능**하다.

![](https://lh3.googleusercontent.com/iJEcNsDn8MYkj8FEQBImN6XdUKp2\_r02BFpL\_cdj0FHwSYif16QXzNcGatNCwUEmqZXfRWu-3KHTOgCMRTL2Dvj5yLtfRPRUW5zLx\_SdWNVFQbshdkT6SQgf-1CYX9YbrwuuHMJj=w640-h213)

#### Ingress

클러스터 외부에서 내부로 접근하는 요청들을 어떻게 처리할지 규칙을 정의하고 규칙에 따라 트래픽을 관리한다. 인그레스는 L7 로드밸런싱 기능으로 **URI 기반 라우팅**을 제공한다.

#### Namespace

논리적으로 분리된 작업그룹으로 **Namespace별로 리소스를 격리**되어 있다.

#### ConfigMap

**환경설정을 저장**하는 오브젝트이다.

#### Secret

컨피그맵에 저장할 수 없는 **민감정보를 저장**하는 오브젝트이다. base64 값으로 저장한다.

#### 오브젝트 템플릿

```yaml
# 여러 개 파일을 포함할 때 구분자
# comment apiVersion: # 쿠버네티스의 API 버전
kind: # 오브젝트 종류 (Pod, Deployment, Service 등)
metadata: # 오브젝트 메타정보 (이름, 레이블 등)
spec: # 파드 컨테이너 생성 정보
```

### **컨트롤러 (Controller)**

컨트롤러는 파드 상태 관리 제공하는 것으로 레플리카셋, 디플로이먼트, 데몬셋, 스테이트풀 셋, 잡 등이 있다.

#### ReplicaSet Controller

**지정한 실행 파드 수를 유지시키는 역할**을 하는 Controller이다. 디플로이먼트가 상위 개념이다.

#### Deployment Controller

**파드를 배포하고를 관리**하는 Controller이다. 쿠버네티스 deployment를 설정하여 애플리케이션 컨테이너를 배포할 수 있다. deployment를 설정하면 replica 값에 따라 파드를 생성하고 관리할 ReplicaSet도 생성한다.

#### DaemonSet Controller

로그 수집기와 같이 **모든 노드에 파드 하나씩 구축할 때** 각 파드를 관리하는 Controller이다.

#### StatefulSet Controller

**스테이트를 가지고 있는 파드들을 관리**하는 Controller이다. 순서를 지정하여 파드를 실행하고 볼륨을 지정하여 파드가 내려가도 데이터를 잃지 않는다. 파드 이름 뒤에 순서를 나타내는 숫자가 붙는다.(0 \~ n)

#### Job Controller

배치 작업 같이 **한번 실행되고 끝나는 작업용 파드를 관리**하는 Controller 이다.

#### CronJob Controller

주기적으로 실행하는 Job을 Controller 한다.

### Label, Selector, Annotation

레이블은 특정 오브젝트 인식표이고, 어노테이션은 오브젝트 주석, 셀렉터는 특정 레이블을 찾는 역할을 한다.

### 쿠버네티스 아키텍처

![](https://lh3.googleusercontent.com/doCy\_9gT\_J5kMrZP6YbgJLmw90XYQGVbcGM2uXyGpVi8wYVDyDofDylZJa9qOjR6mr13BstWsk9K-fc2TkFRkS-h-Oi8T8PvDtSYanoFZZsY0krqTugoaGv5Qe6Dmy5hdoBXt3vY=w640-h495)

### Kubernetes 동작절차

사용자가 API Server에 컨테이너 생성하라고 전달하면 Scheduler가 어느 노드에 생성할지 스케줄링하고 정보를 etcd에 저장하면 kubelet이 컨테이너를 생성한다.

![](<../.gitbook/assets/image (1).png>)

마이크로 서비스 아키텍처가 어려운 점 중의 하나는 서비스가 증가할 수록, 서비스간 연계가 복잡해져서 장애가 발생했을 때 어느 서비스에 장애가 발생했는지 찾기가 어렵다. 그래서 마이크로 서비스에서는 서비스간 모니터링이 중요하다.

### 참고자료

Kubernetes components, [https://kubernetes.io/ko/docs/concepts/overview/components/](https://kubernetes.io/ko/docs/concepts/overview/components/)\
Kubernetes architecture, [https://luludansmarue.github.io/kubernetes-docker-lab/k8s/architecture.html](https://luludansmarue.github.io/kubernetes-docker-lab/k8s/architecture.html)\
Kubernetes Concepts Condensed In A Diagram, [http://www.mycloudreference.com/kubernetes-concepts-condensed-in-a-diagram/](http://www.mycloudreference.com/kubernetes-concepts-condensed-in-a-diagram/)\
Patterns for Composite Containers, [https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/](https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/)\
Kubernetes NodePort vs LoadBalancer vs Ingress? When should I use what?, [https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)\
[https://kubernetes.io/ko/docs/concepts/services-networking/service/#proxy-mode-userspace](https://kubernetes.io/ko/docs/concepts/services-networking/service/#proxy-mode-userspace)\
Kubernetes Ingress, Istio Gateway, [https://binux.tistory.com/63?category=934681](https://binux.tistory.com/63?category=934681)\
[https://medium.com/@zhaohuabing/which-one-is-the-right-choice-for-the-ingress-gateway-of-your-service-mesh-21a280d4a29c](https://medium.com/@zhaohuabing/which-one-is-the-right-choice-for-the-ingress-gateway-of-your-service-mesh-21a280d4a29c)
