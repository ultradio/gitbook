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
다음은 http_requests_total 메트릭의 데이터 모델 정의 예시이다.

|Simple Model                | http_requests_total 5m                          |
|----------------------------|-------------------------------------------------|
|Model with label            |http_requests_total {method="GET"} 5m            |
|Model with label and filter |http_requests_total {job=~".*", method="GET"} 5m |

#### Metric Typpes
Promethues는 4가지 메트릭 타입(Counter, Gauage, Histogram, Summary)을 정의하여 저장한다.

|Counter                     | Gauge                  |Histogram                       |Summary                 |
|----------------------------|------------------------|--------------------------------|------------------------|
|누적개수, 크기              |현재 값, 증가/감소 값    |특정 기간 집계(버킷별)           |특정 기간 집계(버킷별 X)|
|request count, error count  |memory usage, queue size|response size                   | response time          |

#### Prometheus Configuration

Prometheus server 설정은 prometheus.yaml 파일로 정의한다.

메트릭 수집 간격, Alert 설정, 메트릭 집계 규칙, 어떤 exporter로 부터 메트릭을 수집할지 등을 정의한다.

```yaml
# 기본적인 전역 설정
global:
  scrape_interval:     15s # 15초마다 매트릭을 수집한다.
  evaluation_interval: 15s # 15초마다 규칙을 평가한다.
  
# Alertmanager 설정
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093
  
# 규칙을 처음 한번 로딩하고 'evaluation_interval'설정에 따라 정기적으로 규칙을 평가한다.
rule_files:
  - "example_rules.yml"
  # - "test_rules.yml"
  
# 매트릭을 수집할 엔드포인트를 설정. 여기서는 Prometheus 서버 자신을 설정했다.
scrape_configs:
  # 이 설정에서 수집한 타임시리즈에 'job=<job_name>'으로 잡의 이름을 설정한다.
  - job_name: 'prometheus'    # 'metrics_path'라는 설정의 기본 값은 '/metrics'이고 프로토콜 기본값은 'http'이다.
    static_configs:
    - targets: ['localhost:9090']
```

#### PromQL

PromQL은 Prometheus Query Language 이다. \
PromQL을 이용하여 Prometheus server 에서 메트릭을 필터링, 추출, 조회를 할 수 있다.

String, Scalar, Instant Vector, Range Vector로 메트릭을 조회할 수 있다.




### 참고자료 <a href="#prometheus" id="prometheus"></a>
