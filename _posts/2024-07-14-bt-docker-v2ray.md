---
layout:     post
title:      "宝塔+Docker搭建v2ray"
subtitle:   ""
date:       2024-07-14 12:00:00
author:     "8838"
header-img: "img/pic.jpg"
tags:
    - v2ray
    - docker
    - 宝塔
---

准备个域名解析到服务器，安装好宝塔，安装Nginx+Docker，安装完成后回到SSH窗口

拉取镜像

```
docker pull teddysun/v2ray:4.45.2
```

创建文件夹

```
mkdir -p /etc/v2ray
```

写入配置文件(↓整个复制)

```
cat > /etc/v2ray/config.json <<EOF
{
  "inbounds": [
    {
      "port": 10000,
      "listen":"127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "000fe881-b655-4212-b804-b00f9970d5aa",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/happy"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
EOF
```

代码中的000fe881-b655-4212-b804-b00f9970d5aa可以变更一下。比如换几个数字。相当于是个密码。但是格式必须相同(小火箭里的UUID指的就是这串代码)

启动容器

```
docker run -d --network host --name v2ray --restart=always -v /etc/v2ray:/etc/v2ray teddysun/v2ray:4.45.2
```

1,然后宝塔新建一个网站(域名是文章开头你解析的)，如下图所示（数据库，PHP都无所谓）

![](/img/bt-v2/IMG_0211.JPG)


2,首先申请SSL证书(这步不用说了吧)

其实这步也可以省略。反正就是申请了证书后面客户端就用443端口开启tls，不申请就用80端口关闭tls。但是443肯定安全些

3,然后点击配置文件，在这个位置(如下图)添加以下代码

```
    location /happy {
    proxy_pass http://127.0.0.1:10000;
    proxy_http_version  1.1;
    proxy_cache_bypass  $http_upgrade;

    proxy_set_header Upgrade           $http_upgrade;
    proxy_set_header Connection        "upgrade";
    proxy_set_header Host              $http_host;
    proxy_set_header X-Real-IP         $remote_addr;
    proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host  $host;
    proxy_set_header X-Forwarded-Port  $server_port;
    }
```

![](/img/bt-v2/IMG_1456.jpeg)

配置文件最底部的一些代码要删除，否则会报错(如下图圈出来的都删了)

![](/img/bt-v2/IMG_1457.jpeg)


保存，服务端搭建完成

客户端配置参考：[传送](https://8838.github.io/2021/09/13/bt-v2ray/#%E4%B8%8B%E9%9D%A2%E8%AF%B4ios%E5%B0%8F%E7%81%AB%E7%AE%AD%E9%85%8D%E7%BD%AE)

-——

如果你想编辑配置文件(改UUID密码啥的)

```
/etc/v2ray/config.json
```

去宝塔找到这个文件编辑就行。编辑完后需要重启一下docker容器
