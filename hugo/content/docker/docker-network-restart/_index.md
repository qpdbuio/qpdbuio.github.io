+++
title = "重启docker网络"
+++

### docker容器中网络不通的情况下
{{< code >}}
service docker stop
ifconfig docker0 down
brctl delbr docker0
service docker start
{{< /code >}}