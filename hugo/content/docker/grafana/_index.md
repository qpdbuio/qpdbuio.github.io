+++
title = "安装grafana"
weight = 14
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  grafana:
    image: "grafana/grafana:9.5.2"
    container_name: "docker-grafana"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
    ports:
      - "3000:3000"
    volumes:
      - ./grafana/:/var/lib/grafana/:rw
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
