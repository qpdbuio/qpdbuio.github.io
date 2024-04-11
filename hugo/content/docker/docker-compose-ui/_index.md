+++
title = "安装docker-compose-ui"
description = "_index.md"
weight = 3
+++

### 创建指定网络docker-net
{{< code >}}
docker network create docker-net
{{< /code >}}

### docker-compose.yml
{{< code >}}
version: "3"
services:
  tomcat:
    image: "francescou/docker-compose-ui:1.13.0"
    container_name: "docker-compose-ui"
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
    ports:
      - "5000:5000"
    working_dir: "/opt/war/"
    volumes:
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
      - "/usr/share/fonts/:/usr/share/fonts/"
      - "/var/run/docker.sock:/var/run/docker.sock"
      - "/opt/war/:/opt/war/"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}