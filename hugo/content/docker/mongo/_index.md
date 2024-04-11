+++
title = "安装mongo"
weight = 23
+++

### 已创建docker-net网络
### docker-compose.yml
{{< code >}}
version: "3"
services:
  tomcat:
    image: "mongo:latest"
    container_name: "docker-mongo"
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
    ports:
      - "27017:27017"
    volumes:
      - "./data:/data/db"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
