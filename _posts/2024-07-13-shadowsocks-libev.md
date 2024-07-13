---
layout:     post
title:      "Docker快速搭建shadowsocks-libev"
subtitle:   ""
date:       2015-01-29 12:00:00
author:     "8838"
header-img: "img/pic.jpg"
tags:
    - shadowsocks
    - docker
---

本教程由Debian系统搭建

bbr当然是必不可少的，先开启它

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf && echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf && sysctl -p
```

用下面命令验证一下是否成功开启

```
lsmod | grep bbr
```

输出→`tcp_bbr 20480 1`类似值则成功开启了


-——

安装docker

```
wget -qO- get.docker.com | bash
```

开启docker

```
systemctl start docker
```

开启系统自启

```
systemctl enable docker
```

拉取镜像

```
docker pull teddysun/shadowsocks-libev:3.3.5
```

创建文件夹

```
mkdir -p /etc/shadowsocks-libev
```

写入配置文件

```
cat > /etc/shadowsocks-libev/config.json <<EOF
{
    "server": "0.0.0.0",
    "server_port":9001,
    "password":"密码",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":false,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
}
EOF
```

上方`密码`自己自定义了再敲入ssh

启动容器

```
docker run -d -p 9001:9001 -p 9001:9001/udp --name ss-libev --restart=always -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev:3.3.5
```

你会得到一些东西

* 地址：你的ip
* 端口：9001
* 密码：你自定义的
* 算法：aes-256-gcm

## 常用命令

重启容器，一般用于修改了配置文件后

```
docker restart ss-libev
```

停止容器

```
docker stop ss-libev
```

移除容器(移除前需要先停止容器)

```
docker rm ss-libev
```
