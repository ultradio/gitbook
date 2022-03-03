---
description: Prometheus
---

# Prometheus

CNCF 프로젝트 중 하나로 시스템과 서비스를 모니터링하는 도구이다.

Kubernetes 모니터링 도구의 거의 표준이다.

### Prometheus architecture <a href="#prometheus-prometheus" id="prometheus-prometheus"></a>
Prometheus는 Service Discovery, Jobs/Exporters, Pushgateway, Prometheus Server, Alertmanageer 컴포넌트로 구성되어 있다.

각 Prometheus 컴포넌트에 대해 알아보자.


### Jobs/exporters <a href="#prometheus-jobs-exporters" id="prometheus-jobs-exporters"></a>

* 실제로 매트릭을 수집하는 프로세스
* 메트릭 수집 과정
  * exporter가 매트릭을 수집하고 /metrics 라는 HTTP 엔드포인트를 제공
  * Prometheus server가 이 exporter의 엔드포인트로 매트릭 정보를 수집(Pull)
  * 수집한 정보를 조회하여 테이블 및 그래프 형태로 볼 수 있음

### Grafana <a href="#prometheus-grafana" id="prometheus-grafana"></a>

Prometheus 시각화 도구가 부족해서 시각화 도구인 Grafana를 활용

### Prometheus 설치 방법 <a href="#prometheus-prometheus" id="prometheus-prometheus"></a>

Prometheus 서버는 이 yml 파일을 통해 기본적인 설정을 하고 어떤 exporter로부터 매트릭 정보를 수집할 것인지(job\_name)를 확인할 수 있으며,

Prometheus가 15초 마다 매트릭을 수집하는데, `http://localhost:9090/metrics` 에서 제공하는 매트릭 정보를 수집하도록 설정했다. 그리고 이 수집에 대한 Job 이름은 `prometheus`로 지정하였다.

```yaml
# </etc/prometheus/prometheus.yml>
# 기본적인 전역 설정
global:
  scrape_interval:     15s # 15초마다 매트릭을 수집한다. 기본은 1분이다.
  scrape_timeout:      10s # 매트릭 수집 요청 대기시간
  # 'scrpae_timeout' 이라는 설정은 기본적으로 10초로 세팅되어 있다.
 
 
# Alertmanager 설정
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093
 
 
# 규칙을 처음 한번 로딩하고 'evaluation_interval'설정에 따라 정기적으로 규칙을 평가한다.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"
 
 
# 매트릭을 수집할 엔드포인트를 설정. 여기서는 Prometheus 서버 자신을 가리키는 설정을 했다.
scrape_configs:
  # 이 설정에서 수집한 타임시리즈에 'job=<job_name>'으로 잡의 이름을 설정한다.
  - job_name: 'prometheus'    # 'metrics_path'라는 설정의 기본 값은 '/metrics'이고
    # 'scheme'라는 설정의 기본 값은 'http'이다.
    static_configs:
    - targets: ['localhost:9090']
```

#### 어떤  exporter로 부터 매트릭을 수집하는지 확인 <a href="#prometheus-exporter" id="prometheus-exporter"></a>

```shell
/prometheus $ cat /etc/prometheus/config/prometheus.yaml | grep job_name
- job_name: monitoring/alertmanager/0
- job_name: monitoring/coredns/0
- job_name: monitoring/grafana/0
- job_name: monitoring/kube-apiserver/0
- job_name: monitoring/kube-controller-manager/0
- job_name: monitoring/kube-scheduler/0
- job_name: monitoring/kube-state-metrics/0
- job_name: monitoring/kube-state-metrics/1
- job_name: monitoring/kubelet/0
- job_name: monitoring/kubelet/1
- job_name: monitoring/node-exporter/0
- job_name: monitoring/prometheus/0
- job_name: monitoring/prometheus-operator/0
```

#### 데이터를 수집할 exporter의 경로 및 방식 확인 <a href="#prometheus-exporter" id="prometheus-exporter"></a>

```yaml
scrape_configs:
   - job_name: monitoring/node-exporter/0
     metrics_path: '/metrics'  # exporter가 제공하는 엔드포인트가 /metrics라면 metrics_path의 설정을 생략해도 된다.
     honor_labels: false
     kubernetes_sd_configs:
     - role: endpoints
       namespaces:
         names:
         - monitoring
     scrape_interval: 30s
     scheme: https            # exporter의 프로토콜이 http라면 scheme의 설정을 생략해도 된다.
```

### Node Exporter <a href="#prometheus-nodeexporter" id="prometheus-nodeexporter"></a>

curl [http://localhost:9100/metrics](http://localhost:9100/metrics)

kubectl exec -it -n monitoring node-exporter-54d4x sh

```shell
/ $ netstat -alntp | grep 9100
netstat: showing only processes with your user ID
tcp        0      0 172.21.100.85:9100      0.0.0.0:*               LISTEN      24494/kube-rbac-pro
tcp        0      0 127.0.0.1:9100          0.0.0.0:*               LISTEN      24370/node_exporter
tcp        0      0 172.21.100.85:9100      10.244.2.58:34172       ESTABLISHED 24494/kube-rbac-pro
tcp        0      0 172.21.100.85:9100      172.21.100.88:6058      ESTABLISHED 24494/kube-rbac-pro
tcp        0      0 127.0.0.1:34088         127.0.0.1:9100          ESTABLISHED 24494/kube-rbac-pro
tcp        0      0 127.0.0.1:9100          127.0.0.1:34088         ESTABLISHED 24370/node_exporter
/ $ ps -ef | grep 24370
 5971 nobody    0:00 grep 24370
24370 nobody   33:39 /bin/node_exporter --web.listen-address=127.0.0.1:9100 --path.procfs=/host/proc --path.sysfs=/host/sys --path.rootfs=/host/root --collector.filesystem.ignored-mount-points=^/(dev|proc|sys|var/lib/docker/.+)($|/) --collector.filesystem.ignored-fs-types=^(autofs|binfmt_misc|cgroup|configfs|debugfs|devpts|devtmpfs|fusectl|hugetlbfs|mqueue|overlay|proc|procfs|pstore|rpc_pipefs|securityfs|sysfs|tracefs)$
h
```

### 참고자료 <a href="#prometheus" id="prometheus"></a>

[https://medium.com/finda-tech/prometheus%EB%9E%80-cf52c9a8785f](https://medium.com/finda-tech/prometheus%EB%9E%80-cf52c9a8785f)

[https://grafana.com/docs/grafana/latest/variables/templates-and-variables/](https://grafana.com/docs/grafana/latest/variables/templates-and-variables/)

[https://medium.com/@eun9882/node-exporter%EB%A5%BC-%EC%84%A4%EC%B9%98%ED%95%B4-%EB%B3%B4%EC%9E%90-623e554f3d9b](https://medium.com/@eun9882/node-exporter%EB%A5%BC-%EC%84%A4%EC%B9%98%ED%95%B4-%EB%B3%B4%EC%9E%90-623e554f3d9b)\
\
[https://docs.aws.amazon.com/ko\_kr/AmazonCloudWatch/latest/monitoring/ContainerInsights-Prometheus-Setup-configure.html](https://docs.aws.amazon.com/ko\_kr/AmazonCloudWatch/latest/monitoring/ContainerInsights-Prometheus-Setup-configure.html)
