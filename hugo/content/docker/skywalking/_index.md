+++
title = "安装skywalking"
weight = 12
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  skywalking-server:
    image: "apache/skywalking-oap-server:8.8.1"
    container_name: "skywalking-server"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
      - "SW_STORAGE=elasticsearch"
      - "SW_STORAGE_ES_CLUSTER_NODES=192.168.0.1:9200"
      - "JAVA_OPTS=-Xms256m -Xmx256m"
    ports:
      - "11800:11800"
      - "12800:12800"
    networks:
      - docker-net

  skywalking-ui:
    image: "apache/skywalking-ui:8.8.1"
    container_name: "skywalking-ui"
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
      - "SW_OAP_ADDRESS=http://skywalking-server:12800"
      - "JAVA_OPTS=-Xms256m -Xmx256m"
    ports:
      - "28080:8080"
    networks:
      - docker-net
    depends_on:
      - skywalking-server

networks:
  docker-net:
    external: true
{{< /code >}}
