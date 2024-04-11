+++
title = "使用iptables实现转发"
description = "_index.md"
+++

### 说明
- 有2台服务器10.0.0.174和10.0.0.173, 对应的网卡名称都是eth0
- 访问174的3306端口访问173的3306端口

### 启动iptables服务
{{< code >}}
systemctl start  iptables
systemctl enable iptables
systemctl status iptables
service iptables start
chkconfig iptables on
{{< /code >}}

### linux系统
{{< code >}}
net.ipv4.ip_forward = 1
net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 300
net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 120
net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait = 60
net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait = 120
{{< /code >}}

### iptables配置
- 10.0.0.174上iptables配置
{{< code >}}
iptables -A FORWARD -m state --state ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp --dport 3306 -m state --state NEW -j ACCEPT
iptables -A FORWARD -p tcp --dport 3306 -m state --state NEW -j ACCEPT
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 3306 -j DNAT --to-destination 10.0.0.173:3306
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 10.0.0.174
{{< /code >}}
- 10.0.0.173上iptables配置
{{< code >}}
iptables -A FORWARD -m state --state ESTABLISHED -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 10.0.0.173
{{< /code >}}

### 重启iptables
{{< code >}}
systemctl restart iptables
{{< /code >}}
