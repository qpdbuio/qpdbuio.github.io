+++
title = "安装elasticsearch"
weight = 11
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: '3'
services:
  elastic:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.8.23
    container_name: elasticsearch
    restart: "always"
    environment:
      - "TZ=Asia/Shanghai"
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - "./data:/usr/share/elasticsearch/data"
      - "./config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### elasticsearch.yml
{{< code >}}
cluster.name: "my-application"
network.host: 0.0.0.0

discovery.zen.minimum_master_nodes: 1
xpack.license.self_generated.type: basic
{{< /code >}}