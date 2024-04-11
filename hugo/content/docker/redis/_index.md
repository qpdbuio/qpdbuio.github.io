+++
title = "安装redis"
weight = 7
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: '3'
services:
  #redis容器
  redis:
    #定义主机名
    container_name: redis
    #使用的镜像
    image: redis:5.0.2
    #容器的映射端口
    ports:
      - "6379:6379"
    command: redis-server /etc/redis/6379.conf
    #定义挂载点
    volumes:
      - ./data:/data
      - ./conf:/etc/redis/
    #环境变量
    privileged: true
    environment:
      - TZ=Asia/Shanghai
      - LANG=en_US.UTF-8
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### 6379.conf
{{< code >}}
port 6379
bind 127.0.0.1
databases 16
requirepass q1w2e3r4
{{< /code >}}