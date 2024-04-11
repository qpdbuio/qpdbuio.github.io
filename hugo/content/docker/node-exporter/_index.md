+++
title = "安装node-exporter"
weight = 13
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  node-exporter:
    image: "bitnami/node-exporter:1.6.0"
    container_name: "node-exporter"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
    ports:
      - "9100:9100"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
