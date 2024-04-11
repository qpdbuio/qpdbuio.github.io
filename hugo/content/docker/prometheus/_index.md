+++
title = "安装prometheus"
weight = 15
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  prometheus:
    image: "bitnami/prometheus:2.44.0"
    container_name: "docker-prometheus"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
    ports:
      - "9090:9090"
    volumes:
      - "./prometheus.yml:/etc/prometheus/prometheus.yml"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### prometheus.yml
{{< code >}}
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
     - targets: ['192.168.0.1:9090']

  - job_name: '192.168.0.1'
    static_configs:
     - targets: ['192.168.0.1:9100']
{{< /code >}}