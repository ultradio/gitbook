---
description: Kubernetes Authentication, Authorization
---

# Kubernetes Authentication

보안관리 보통 2가지가 있다. 클라이언트 접속 허가 관리하는 **인증관리**와 접속이 허가된 클라이언트에 대한 리소스 접근 권한을 관리하는 **권한 관리**가 있다.

### Authentication

쿠버네티스에서 인증방법으로\
**kubeconfig** 파일로 인증,\
ServiceAccount **Token**을 이용한 인증,\
컨테이너에 가지고 있는 **Token**을 이용한 인증,\
**idP**를 통한 인증이 있다.

#### **kubeconfig 파일로 인증**

kubectl로 API를 호출할 때, kubeconfig 파일로 인증하고 클러스터에 접근할 수 있다.

```python
from kubernetes import client, config

config.load_kube_config()

v1=client.CoreV1Api()
print("Listing pods with their IPs:")
ret = v1.list_pod_for_all_namespaces(watch=False)
```

#### **ServiceAccount 토근을 이용하여 인증**

파드가 API를 호출할 때 ServiceAccount 토큰을 이용하여 인증한다.

![](https://lh3.googleusercontent.com/-J\_OJlc64iFI/YT8CxknqpYI/AAAAAAAAZ-g/SRXJuk\_cOsQ5WjuF9847gy0ay\_8bPNw7ACLcBGAsYHQ/w640-h344/image.png)

#### **컨테이너에 위치한 토큰을 이용하여 인증**

쿠버네티스 클라이언트를 사용할 때, 파드 내 컨테이너에 위치한 토큰을 이용하여 인증한다.

```python
configuration = config.load_incluster_config()
 
def load_incluster_config():
    InClusterConfigLoader(token_filename=SERVICE_TOKEN_FILENAME,
                          cert_filename=SERVICE_CERT_FILENAME).load_and_set()
 
SERVICE_TOKEN_FILENAME = "/var/run/secrets/kubernetes.io/serviceaccount/token"
SERVICE_CERT_FILENAME = "/var/run/secrets/kubernetes.io/serviceaccount/ca.crt"
```

#### **idP를 통한 인증**

쿠버네티스는 사용자를 API로 직접 관리하지 않고 Dex와 연계하여 기존 시스템(LDAP 등)을 활용하여 인증할 수 있다. Dex는 서드파티로 부터 OAuth 인증을  관리하는 도구이다. Dex는 OAuth 서드파티와의 중간 매개체 역할을 해주기 때문에 OAuth의 인증 토큰 발급, 저장 및 관리를 좀더 쉽게 할 수 있다.

![](https://lh3.googleusercontent.com/-pljSyqUavV8/YT8ET8BjzcI/AAAAAAAAZ-o/nPXUtBVyGfMt0KkBpM65iDc8qLcGwGFwgCLcBGAsYHQ/w640-h308/image.png)

### **Authorization**

쿠버네티스는 RBAC을 사용하여 리소스 권한을 관리한다.\
어카운트는 유저 어카운트와 시스템 어카운트로 구분되는데, 유저 어카운트는 User와 User를  묶은 Group으로 정의하고, 시스템 어카운트는 ServiceAccount 로 정의한다. 어카운트의 리소스 권한(예, Pod - create/list/delete)은 Role에 정의하며, Role을 어카운트에 부여할 때 RoleBinding을 설정한다.

![](<../.gitbook/assets/image (3).png>)

#### **ClusterRole과 Role**

Role은 적용 범위에 따라 ClusterRole과 Role로 구분한다. **ClusterRole은 클러스터 전체 리소스 권한을 정의**하고, **Role은 특정 네임스페이스 내의 리소스 권한을 정의**한다.

#### **Namespace**

쿠버네티스는 namespace를 사용하여 리소스를 격리하고, 클러스터 내에서 여러 사용자들 자원을 구분해서 나눠 쓸 수 있다. 동일한 namespace 내에서 리소스는 레이블을 사용하여 구분한다.

### 참고자료

[https://lcc3108.github.io/articles/2020-12/Istio+Dex-인증](https://lcc3108.github.io/articles/2020-12/Istio+Dex-%EC%9D%B8%EC%A6%9D)\
Istio Usage in Kubeflow, [https://www.kubeflow.org/docs/other-guides/istio-in-kubeflow/](https://www.kubeflow.org/docs/other-guides/istio-in-kubeflow/)\
[https://speakerdeck.com/chanyilin/authz?slide=15](https://speakerdeck.com/chanyilin/authz?slide=15)\
[https://speakerdeck.com/chanyilin/authz?slide=12](https://speakerdeck.com/chanyilin/authz?slide=12)\
[https://medium.com/kubeflow/enabling-kubeflow-with-enterprise-grade-auth-for-on-premise-deployments-ae7dd13a69e5](https://medium.com/kubeflow/enabling-kubeflow-with-enterprise-grade-auth-for-on-premise-deployments-ae7dd13a69e5)\
[https://waspro.tistory.com/608](https://waspro.tistory.com/608)\
[https://bcho.tistory.com/1272](https://bcho.tistory.com/1272)
