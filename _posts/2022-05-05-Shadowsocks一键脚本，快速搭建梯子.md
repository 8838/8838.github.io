---
layout:     post
title:      "Shadowsocks一键脚本，快速搭建梯子"
subtitle:   ""
date:       2022-05-05 12:00:00
author:     "代码"
header-img: "img/pic.jpg"
tags:
    - shadowsocks
---

前言：有时候我们刚买的vps要测试梯子速度，或者临时梯子。这时候shadowsocks一键脚本就派上用场了

vps ssh窗口直接敲代码

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh && chmod +x shadowsocks-all.sh && ./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

![](https://www.moe.ms/usr/uploads/2022/10/2749129845.jpeg)

* `4`是协议，一般我们选择第4种
* `123456`是密码，自己定义就好
* `80`是端口，1-65535随意填写，不要填写被占用的，例如22是我们ssh的端口
* `3`是加密方式，有些加密方式某些客户端不支持。建议前3种任选其一
* `n`表示不开启插件，这个很麻烦还要其他配置。千万不要输入y

然后一路回车，最后代码跑完了就会显示ss信息，截个图保存啥的输入到客户端就OK！
