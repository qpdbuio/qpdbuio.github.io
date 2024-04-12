+++
title = "安装jenkins"
weight = 24
+++

### 宿主机需安装及路径
- docker: /usr/bin/docker
- docker-compose: /usr/local/bin/docker-compose
- java: /export/local/java
- maven: /export/local/maven

### 已创建docker-net网络
### docker-compose.yml
{{< code >}}
version: '3'
services:
  jenkins:
    image: 'jenkins/jenkins:lts-jdk8'
    container_name: jenkins
    restart: always
    environment:
      - TZ=Asia/Shanghai
    ports:
      - '8080:8080'
    volumes:
      - '/export/jenkins_home:/var/jenkins_home'
      - '/export/local/java/jdk1.8:/usr/java/jdk1.8'
      - '/export/local/maven:/usr/local/maven'
      - '/var/run/docker.sock:/var/run/docker.sock'
      - '/usr/bin/docker:/usr/bin/docker'
      - '/usr/local/bin/docker-compose:/usr/local/bin/docker-compose'

    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
