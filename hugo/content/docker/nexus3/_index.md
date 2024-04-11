+++
title = "安装nexus3"
weight = 5
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  nexus:
    image: sonatype/nexus3
    container_name: nexus
    restart: always
    ports:
      - "8081:8081"
    volumes:
      - "./nexus-data:/nexus-data:rw"
      - "./sonatype-work:/opt/sonatype/sonatype-work:rw"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}