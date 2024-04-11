+++
title = "安装loki"
weight = 17
+++

### 收集日志
### 已创建docker-net
### docker-compose.yml
{{< code >}}
services:
  loki:
    image: grafana/loki:2.8.0
    container_name: "docker-grafana-loki"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml
    volumes:
      - ./loki/:/loki/:rw
      - ./loki-config.yaml:/etc/loki/local-config.yaml
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### loki-config.yaml
{{< code >}}
auth_enabled: false

server:
  http_listen_port: 3100

common:
  path_prefix: /loki
  storage:
    filesystem:
      chunks_directory: /loki/chunks
      rules_directory: /loki/rules
  replication_factor: 1
  ring:
    kvstore:
      store: inmemory

schema_config:
  configs:
    - from: 2023-01-01
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h

limits_config:
  ingestion_rate_strategy: global
  ingestion_rate_mb: 50
  ingestion_burst_size_mb: 50

ruler:
  alertmanager_url: http://localhost:9093
{{< /code >}}