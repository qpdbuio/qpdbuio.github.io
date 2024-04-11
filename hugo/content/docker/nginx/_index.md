+++
title = "安装nginx"
weight = 6
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  nginx:
    image: "nginx:1.14.2"
    container_name: "test-nginx"
    restart: "always"
    environment:
      - TZ=Asia/Shanghai
    ports:
      - "80:80"
    volumes:
      - "./conf.d/:/etc/nginx/conf.d/"
      - "./www/:/usr/share/nginx/html/"
      - "./logs/:/var/log/nginx/"
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}