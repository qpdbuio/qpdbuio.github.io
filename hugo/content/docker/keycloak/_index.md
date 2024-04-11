+++
title = "安装keycloak"
weight = 19
+++

### 已创建docker-keycloak
### docker-compose.yml
{{< code >}}
version: "3"
services:
  kong:
    image: keycloak/keycloak:23.0.1
    container_name: docker-keycloak
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
      - DEBUG_MODE=true
      - PRINT_ENV=true
      - KEYCLOAK_ADMIN=admin
      - KEYCLOAK_ADMIN_PASSWORD=admin
    ports:
      - 38080:8080
    command: start-dev
    volumes:
      - "/etc/localtime:/etc/localtime:ro"
    networks:
      - docker-keycloak
networks:
    docker-keycloak:
        external: true
{{< /code >}}
