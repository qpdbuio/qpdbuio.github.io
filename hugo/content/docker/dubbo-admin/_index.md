+++
title = "安装dubbo-admin"
weight = 9
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: '3'
services:
  dubbo-admin:
    image: apache/dubbo-admin:0.5.0
    container_name: dubbo-admin
    restart: "always"
    ports:
      - "18888:8080"
    environment:
      - "JAVA_OPTS=-Xms512m -Xmx512m"
      - "admin.registry.address=nacos://10.0.0.1:8848?username=nacos&password=nacos"
      - "admin.config-center=nacos://10.0.0.1:8848?username=nacos&password=nacos"
      - "admin.metadata-report.address=nacos://10.0.0.1:8848?username=nacos&password=nacos"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}