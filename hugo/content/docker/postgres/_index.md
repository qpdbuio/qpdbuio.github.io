+++
title = "安装postgres"
weight = 21
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: '3'
version: "3"
services:
  postgres:
    image: postgres
    container_name: postgres-5432
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
    ports:
      - 5432:5432
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
      - "/usr/share/fonts/:/usr/share/fonts/"
      - "./postgresql/data/:/var/lib/postgresql/data"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}
