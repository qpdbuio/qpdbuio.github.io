+++
title = "安装apisix"
weight = 18
+++

### 网关
### 已创建docker-apisix
### docker-compose.yml
{{< code >}}
version: "3"
services:
  apisix:
    image: apache/apisix:3.7.0-debian
    container_name: apache-apisix
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - ALLOW_NONE_AUTHENTICATION=yes
      - ETCD_HOST=etcd
    ports:
      - 9080:9080
      - 9091:9091
      - 9443:9443
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
      - "./config.yaml:/usr/local/apisix/conf/config.yaml"
    networks:
      - docker-apisix
    depends_on:
      - etcd

  apisix-dashboard:
    image: apache/apisix-dashboard:3.0.1-centos
    container_name: apache-apisix-dashboard
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
    ports:
      - 19000:9000
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
      - "./conf.yaml:/usr/local/apisix-dashboard/conf/conf.yaml"
    networks:
      - docker-apisix

  etcd:
    image: bitnami/etcd:3.4.9
    container_name: apache-apisix-etcd
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - ALLOW_NONE_AUTHENTICATION=yes
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
    networks:
      - docker-apisix

networks:
    docker-apisix:
        external: true
{{< /code >}}
