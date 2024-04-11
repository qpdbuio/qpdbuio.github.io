+++
title = "安装docker-compose"
weight = 2
+++

### 已安装docker
{{< code >}}
curl -L https://get.daocloud.io/docker/compose/releases/download/1.12.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
{{< /code >}}

### docker-compose使用
- docker-compose启动, -d参数是后台
{{< code >}}
docker-compose -f docker-compose.yaml up -d
{{< /code >}}
- docker-compose停止
{{< code >}}
docker-compose -f docker-compose.yaml down
{{< /code >}}
- docker-compose日志
{{< code >}}
docker-compose -f docker-compose.yaml logs -f
{{< /code >}}