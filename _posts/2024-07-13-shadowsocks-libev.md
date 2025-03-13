---
layout:     post
title:      "Docker快速搭建shadowsocks-libev"
subtitle:   ""
date:       2024-07-13 12:00:00
author:     "8838"
header-img: "img/pic.jpg"
tags:
    - shadowsocks
    - docker
---

本教程使用的Debian系统，适合新手，无脑敲命令就行

安装docker

```
wget -qO- get.docker.com | bash
```

启动docker

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

写入配置文件(↓整个复制)

```
cat > /etc/shadowsocks-libev/config.json <<EOF
{
    "server":["[::0]", "0.0.0.0"],
    "server_port":9001,
    "password":"8838.github.io",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":false,
    "nameserver":"1.1.1.1",
    "mode":"tcp_and_udp"
}
EOF
```

启动容器

```
docker run -d -p 9001:9001 -p 9001:9001/udp --name ss-libev --restart=always -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev:3.3.5
```

搭建完成，你会得到一些东西

* 地址：你的ip
* 端口：9001
* 密码：8838.github.io
* 算法：aes-256-gcm

-——

如果你想编辑配置文件(改密码啥的)

```
nano /etc/shadowsocks-libev/config.json
```

编辑完后`ctl+x`保存，`y`确认，`回车`退出，记得重启容器↓

## 常用命令

重启容器，一般用于修改了配置文件后

```
docker restart ss-libev
```

启动容器

```
docker start ss-libev
```

停止容器

```
docker stop ss-libev
```

移除容器(移除前需要先停止容器)

```
docker rm ss-libev
```
