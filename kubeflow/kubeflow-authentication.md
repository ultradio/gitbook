---
description: Kubeflow Authentication, Profile, Istio, Dex
---

# Kubeflow Authentication

### Kubeflow에서 Istio 를 통한 사용자 인증 및 리소스 권한 관리

쿠버네티스는 이스티오와 연동하여 사용자 인증와 리소스 권한 관리를 수행하여 사용 요청을 처리할 수 있다.&#x20;

다음 워크플로우 예시는 사용자가 요청한 Kubeflow Central Dashboard에서 노트북 서버 생성 요청을 처리하는 과정이다.

![](https://lh3.googleusercontent.com/-BbIzOgwngcU/YT8LAU-PkEI/AAAAAAAAZ-4/apGilj\_JXTw1oRsjYnfUCgIlLP-IJ4QXACLcBGAsYHQ/w640-h332/image.png)



1\) 로그인 하지 않은 사용자가 브라우저를 Kubeflow에 접속하면, Dex를 통해 idP로 Redirection 한다.\
2\) 사용자가 로그인하면, 쿠키가 브라우저에 저장하고, \
사용자의 Request를 Istio Gateway에서 JWT(JSON Web Token)를 확인한 후 idP에 인증을 요청한다. (인증은 한번만 수행된다.)\
3\) Istio RBAC은 Request 접근권한(Service, Namespace)을 확인한다.\
4\) Request 접근권한이 확인되면, Request를 해당 Controller에 전달한다.\
5\) Controller는 Kubernetes RBAC에서 인증받고 사용자 요청을 처리한다.

**모든 요청은 AuthService(Authorization)로 전달**한다.&#x20;

![](https://lh3.googleusercontent.com/-akdyjCvRcgs/YT8MPcypiQI/AAAAAAAAZ\_A/dFhmZiqaLDsiiNzyEjtVWUw8emGfu7kkwCLcBGAsYHQ/w640-h142/image.png)

### **Istio RBAC**

ServiceRole과 ServiceRoleBinding으로 리소스 권한을 정의한다.

### 용어

Kubeflow Profile\
Kubeflow Profile CRD는 사용자 별 리소스 권한을 관리하기 위해 고안되었다. 쿠버네티스에서 사용자 리소스를 격리하는 개념인 네임스페이스의 레퍼이다. **Profile 마다 하나의 Namespace를 가질 수 있고, Profile 소유자는 다른 사용자와 공유할 수 있다.**

![사진출처: https://www.kubeflow.org/docs/components/multi-tenancy/design/](https://lh3.googleusercontent.com/--c\_Fz2kNxAw/YT8O8DsilVI/AAAAAAAAZ\_I/RkYZZd1VI6cgcOyBITSwwGj08lrri11-gCLcBGAsYHQ/w640-h630/image.png)

### 참고자료

[https://speakerdeck.com/chanyilin/authz?slide=12](https://speakerdeck.com/chanyilin/authz?slide=12)\
[https://medium.com/kubeflow/enabling-kubeflow-with-enterprise-grade-auth-for-on-premise-deployments-ae7dd13a69e5](https://medium.com/kubeflow/enabling-kubeflow-with-enterprise-grade-auth-for-on-premise-deployments-ae7dd13a69e5) \
[https://www.kubeflow.org/docs](https://www.kubeflow.org/docs/components/multi-tenancy/design/)
