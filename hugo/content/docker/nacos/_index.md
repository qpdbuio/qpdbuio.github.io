+++
title = "安装nacos"
weight = 10
+++

MySQL数据库[nacos.sql](./nacos.sql)

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "2"
services:
  nacos:
    image: nacos/nacos-server:${NACOS_VERSION}
    container_name: nacos-standalone-mysql
    restart: "always"
    env_file:
      - ./.env
      - ./nacos-standlone-mysql.env
    volumes:
      - ./logs/:/home/nacos/logs
      - ./init.d/custom.properties:/home/nacos/init.d/custom.properties
    ports:
      - "8848:8848"
      - "9848:9848"
      - "9555:9555"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### .env
{{< code >}}
NACOS_VERSION=v2.2.2
{{< /code >}}

### nacos-standlone-mysql.env
{{< code >}}
PREFER_HOST_MODE=hostname
MODE=standalone
SPRING_DATASOURCE_PLATFORM=mysql
MYSQL_SERVICE_HOST=10.0.0.1
MYSQL_SERVICE_DB_NAME=nacos
MYSQL_SERVICE_PORT=3306
MYSQL_SERVICE_USER=nacos
MYSQL_SERVICE_PASSWORD=nacos
MYSQL_SERVICE_DB_PARAM=characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useSSL=false

NACOS_AUTH_ENABLE=true
NACOS_AUTH_USER_AGENT_AUTH_WHITE_ENABLE=false
NACOS_AUTH_IDENTITY_KEY=nacos
NACOS_AUTH_IDENTITY_VALUE=nacos
NACOS_AUTH_TOKEN=SecretKey01234567890123456789012345678901234567890123456789012345678
{{< /code >}}