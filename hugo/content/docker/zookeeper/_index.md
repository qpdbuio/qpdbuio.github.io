+++
title = "安装zookeeper"
weight = 20
+++

### 已创建docker-zookeeper
### docker-compose.yml
{{< code >}}
version: '3'
services:
  zookeeper:
    image: zookeeper:3.5
    restart: always
    ports:
      - 2181:2181
    logging:
      driver: "json-file"
      options:
        max-size: "10k"
        max-file: "10"
    networks:
      - docker-zookeeper
  web:
    image: elkozmon/zoonavigator-web:0.5.0
    container_name: zoonavigator-web
    ports:
     - "18000:18000"
    environment:
      WEB_HTTP_PORT: 18000
      API_HOST: "api"
      API_PORT: 6000
    depends_on:
     - api
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "10k"
        max-file: "10"
    networks:
      - docker-zookeeper
  api:
    image: elkozmon/zoonavigator-api:0.5.0
    container_name: zoonavigator-api
    environment:
      API_HTTP_PORT: 6000
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "10k"
        max-file: "10"
    networks:
      - docker-zookeeper
  admin:
    image: cao2068959/dubbo-admin:2.7
    restart: "always"
    depends_on:
      - zookeeper
    ports:
      - 8081:8080
    command: -Xms1024m -Xmx1024m -Xmn200m
    volumes:
      - "./myapplication.properties:/dubbo-admin/myapplication.properties:ro"
    environment:
      - admin.registry.address=zookeeper://zookeeper:2181
      - admin.config-center=zookeeper://zookeeper:2181
      - admin.metadata-report.address=zookeeper://zookeeper:2181
      - admin.metadata-report.address=zookeeper://zookeeper:2181
    networks:
      - docker-zookeeper

networks:
  docker-zookeeper:
    external: true
{{< /code >}}
