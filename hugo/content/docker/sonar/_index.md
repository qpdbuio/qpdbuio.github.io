+++
title = "安装sonar"
weight = 22
+++

### 已创建docker-net网络
### 已启动postgres数据库
### docker-compose.yml
{{< code >}}
version: '3'
services:
  sonarqube:
    image: "sonarqube:7.6-community"
    container_name: "sonarqube-76-9000"
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
      - SONARQUBE_JDBC_URL=jdbc:postgresql://postgres:5432/sonar
    ports:
      - "9000:9000"
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
      - "/usr/share/fonts/:/usr/share/fonts/"
      - "./sonarqube/logs/:/opt/sonarqube/logs"
      - "./sonarqube/data/:/opt/sonarqube/data"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
