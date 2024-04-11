+++
title = '基于Linux下安装docker'
weight = 1
+++

### 关闭防火墙
- firewalld
{{< code >}}
systemctl stop firewalld
systemctl disable firewalld
chkconfig firewalld off
{{< /code >}}

- iptables
{{< code >}}
systemctl stop iptables
systemctl disable iptables
chkconfig iptables off
{{< /code >}}

### linux系统
{{< code >}}
net.ipv4.ip_forward = 1
net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 300
net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 120
net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait = 60
net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait = 120
{{< /code >}}

### 已安装yum和yum-utils
### 添加docker的yum源
{{< code >}}
sudo yum-config-manager --add-repo https://download.daocloud.io/docker/linux/centos/docker-ce.repo
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
{{< /code >}}

### yum安装
{{< code >}}
sudo yum install -y  docker-ce.*
sudo systemctl enable docker
sudo systemctl start docker
sudo service docker status
{{< /code >}}