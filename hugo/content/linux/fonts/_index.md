+++
title = "使用yum安装字体库"
description = "_index.md"
weight = 2
+++

### 安装字体库（安装后会出现文件夹 /usr/share/fonts 和 /usr/share/fontconfig ）
{{< code >}}
yum -y install fontconfig  mkfontscale
{{< /code >}}

### 字体扩展
{{< code >}}
mkfontscale
{{< /code >}}

### 新增字体目录
{{< code >}}
mkfontdir
{{< /code >}}

### 刷新缓存
{{< code >}}
fc-cache -fv
{{< /code >}}

### 命令查看已经安装的字体
{{< code >}}
fc-list
{{< /code >}}