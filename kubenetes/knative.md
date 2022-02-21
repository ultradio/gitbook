---
description: Knative
---

# Knative

## Knative

쿠버네티스에서 웹서비스를 구축하려면, Deployment, Ingress, Service  등 리소스를 정의해서 배포해야 한다.  하지만 리소스가 많아질 수록 복잡해지는데, Knative는 Knative CRD에 컨테이너 이름과 컨테이너 이미지 경로 정도만 정의해도, 쉽고 빠르게 **서버리스 서비스**를 구축할 수 있다.

Knative는 Knative Serving, Knative Eventing으로 이루어져 있다.

![](<../.gitbook/assets/image (37).png>)

### Kubernetes Application 배포

Pod, Service, Ingress 작성하여 배포한다.

{% tabs %}
{% tab title="app.py" %}
{% code title="https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html" %}
```python
import os

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
target = os.environ.get('TARGET', 'World')
return 'Hello {}!\n'.format(target)

if __name__ == "__main__":
app.run(debug=True,host='0.0.0.0', \

port=int(os.environ.get('PORT', 8080)))
```
{% endcode %}
{% endtab %}

{% tab title="Dockerfile" %}
```
FROM python:3.7-slim

ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./

RUN pip install Flask gunicorn

CMD exec gunicorn --bind :$PORT \

--workers 1 --threads 8 --timeout 0 app:app
```
{% endtab %}

{% tab title="Pod" %}
```yaml
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple"
```
{% endtab %}

{% tab title="Service" %}
```yaml
kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - port: 5678 # Default port for image
```
{% endtab %}

{% tab title="Ingress" %}
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 5678
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 5678
```
{% endtab %}
{% endtabs %}

### Knative Application 배포

Knative Service CRD를 작성하여 배포한다.

{% tabs %}
{% tab title="app.py" %}
{% code title="https://github.com/knative/docs/tree/release-0.26/docs/serving/samples/hello-world/helloworld-python" %}
```python
import os

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    target = os.environ.get('TARGET', 'World')
    return 'Hello {}!\n'.format(target)

if __name__ == "__main__":
    app.run(debug=True,host='0.0.0.0',port=int(os.environ.get('PORT', 8080)))
```
{% endcode %}
{% endtab %}

{% tab title="Dockerfile" %}
```
FROM python:3.7-slim

ENV PYTHONUNBUFFERED True

ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./

RUN pip install Flask gunicorn

CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 app:app
```
{% endtab %}

{% tab title="service.yaml" %}
```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: helloworld-python
  namespace: default
spec:
  template:
    spec:
      containers:
      - image: docker.io/{username}/helloworld-python
        env:
        - name: TARGET
          value: "Python Sample v1"
```
{% endtab %}
{% endtabs %}

### Knative 구성요소

![](<../.gitbook/assets/image (34).png>)

#### Service

**Knative Application을 정의**한다. Route, Configuration 리소스를 관리한다. Knative Service는 **파드 스펙을 정의**하고 트래픽을 최신 Revision으로 라우팅하거나 고정한 Revision으로 라우팅 하도록 **라우팅 정책**을 정의 할 수 있다.

#### Route

Knative Service로 들어오는 트래픽을 **Revision으로 라우팅**하는 역할을 한다. Knative Service에 정의한 라우팅 정책을 참조하여 Route를 생성한다.&#x20;

#### Configuration

Knative Serving으로 배포할 파드를 정의한다. **Knative Service에 정의한 파드 스펙**으로 컨테이너 경로, 환경변수, 오토스케일링 설정 등을 참조하여 Configuration을 생성한다.

#### Revision

**Configuration의 히스토리**로 Configuration이 새로 생성되거나 Configuration 변경될 때 마다 Configuration을 Snapshot하여 Revision으로 남긴다. Route에서 endpoint를 지정하지 않은 Revision은 자동폐기 되며, 쿠버네티스 리소스도 삭제된다.

![출처: https://knative.dev/docs/serving/](<../.gitbook/assets/image (33).png>)

### Knative Serving

Knative는 **트래픽이 들어오면 Knative Serving Activator가 Pod가 없는 Revision을 확인하고 AutoScaler가 파드를 생성**한다. 이후 Pod가 트래픽을 받을 준비가 되면, Ingress Gateway를 통해 트래픽을 Pod에 전달한다.

![출처: https://knative.dev/docs/serving/istio-authorization/](<../.gitbook/assets/image (35).png>)

1\) Ingress Gateway에 요청이 들어오면 Activator에 전달한다.\
2\) Activator는 요청을 Buffer에 넣는다.\
3\) Buffer에 대기 중인 요청을 PodAutoscaler에 전달한다.\
4\) PodAutoscaler는 Pod가 0개면, 1개를 생성하라는 요청을 Knative Serving에게 전달한다.\
5\) Activator가 Knative Serving에게 Pod가 생성되었는지 확인한다.\
6\) Pod가 없으면, Knative Serving은 Pod를 생성한다.\
7\) Pod가 있으면, 요청을 Buffer에서 꺼내서 Proxy에 전달한다.\
8\) Proxy는 요청을 Pod로 전달하고 Pod에서 응답을 받는다.\
9\) Proxy가 받은 응답을 Ingress에 보내고 Client에게 최종 전달한다.

### Knative Eventing

Event를 produce/consume 하는 방법을 제공하며, Github, AWS SQS 등 다양한 Source와 Kafka 등 Event Channel을 제공한다.

### 참고자료

[https://knative.dev/docs/serving/](https://knative.dev/docs/serving/)\
[https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md](https://github.com/knative/specs/blob/main/specs/serving/knative-api-specification-1.0.md)\
[https://www.researchgate.net/figure/Knative-Serving-Architecture\_fig1\_336567672](https://knative.dev/docs/serving/istio-authorization/)\
[https://livebook.manning.com/book/knative-in-action/chapter-2/v-3/156](https://livebook.manning.com/book/knative-in-action/chapter-2/v-3/156)\
[https://www.slideshare.net/JinwoongKim8/knative](https://www.slideshare.net/JinwoongKim8/knative)
