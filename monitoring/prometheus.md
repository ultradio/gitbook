---
description: Prometheus
---

# Prometheus

CNCF 프로젝트 중 하나로 시스템과 서비스를 모니터링하는 도구이다.

Kubernetes 모니터링 도구의 거의 표준이다.

### Prometheus architecture <a href="#prometheus-prometheus" id="prometheus-prometheus"></a>

Prometheus는 Service Discovery, Jobs/Exporters, Pushgateway, Prometheus Server, Alertmanageer 컴포넌트로 구성되어 있다.

각 Prometheus 컴포넌트에 대해 알아보자.

#### Service discovery <a href="#prometheus-prometheus" id="prometheus-prometheus"></a>

Prometheus는 \*Service Discovery와 연동하여\
Pod가 삭제되거나 추가되는 등 변경사항이 발생하면 자동으로 탐지하여 메트릭을 수집한다.

* 쿠버네티스 Service Discovery는 CoreDNS에 서비스 Domain Name을 등록하고, 동록한 서비스 목록과 IP, 포트를 찾아주는 역할을 한다.

#### Jobs/exporters <a href="#prometheus-jobs-exporters" id="prometheus-jobs-exporters"></a>

Jobs/exporters는 실제로 메트릭을 수집하는 프로세스이다.

메트릭 수집 과정은\
exporter가 메트릭을 수집하고 /metrics 라는 HTTP endpoint를 제공한다.\
Prometheus Server는 exporter에서 제공하는 엔드포인트로 접속하여 Pulling 방식으로 메트릭을 수집한다. 그리고, 시각화 도구를 사용하여 수집한 메트릭을 조회하고 테이블이나 그래프 형태로 시각화할 수 있다.

#### Pushgateway <a href="#prometheus-pushgateway" id="prometheus-pushgateway"></a>

Pushgateway는 Push 방식으로 메트릭을 수집해야 하는 경우에 사용한다.

수집 에이전트가 메트릭을 Pushgateway에 Push하면, Prometheus가 Pushgateway에서 메트릭을 Pulling 하는 방식이다.

#### Prometheus server <a href="#prometheus-server" id="prometheus-server"></a>

Prometheus server는 Retrieval, TSDB, HTTP server 모듈로 구성되어 있다.

Retrieval은 Service discovery에서 모니터링 대상 목록을 가져와서\
모니터링 대상으로 부터 메트릭을 수집하고 TSDB (시계열 DB)에 저장하는 역할을 한다.\
TSDB에 저장한 메트릭은 HTTP server를 통해 PromQL로 메트릭을 조회할 수 있다.

#### Alertmanager <a href="#prometheus-alertmanager" id="prometheus-alertmanager"></a>

Prometheus server로 부터 Alert를 전달 받아 적절한 형식으로 변경하고, 변경한 Alert를 이메일 등으로 전송해주는 역할을 한다.

### Prometheus Data & Model

#### Data Model <a href="#prometheus-data-model" id="prometheus-data-model"></a>

Prometheus 메트릭은 라벨과 데이터 필터를 사용하여 다양한 메트릭을 만들 수 있다.

다음은 메트릭의 데이터 모델 포맷이다.

```yaml
<metric_name>{<label name>={label value}, ...}
```





### 참고자료 <a href="#prometheus" id="prometheus"></a>
