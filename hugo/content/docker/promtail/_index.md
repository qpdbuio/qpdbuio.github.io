+++
title = "安装promtail"
weight = 16
+++

### 收集日志
### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  promtail:
    image: grafana/promtail:2.8.0
    volumes:
      - ./promtail-config.yml:/etc/promtail/config.yml
      - /opt/war:/opt/war
    command: -config.file=/etc/promtail/config.yml
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### promtail-config.yml
{{< code >}}
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://192.168.0.1:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /opt/war/test-*/logs/*-server.log
{{< /code >}}