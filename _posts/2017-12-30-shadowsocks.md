---
layout: post
title: Shadowsocks
date: 2017-12-30 10:43:00 +0800
categories:
- Tech
tags:
- Shadowsocks

---

> 本文内容均援引自网络，仅作学习用途。

## Shadowsocks安装

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

- 启动：/etc/init.d/shadowsocks start
- 修改配置文件：`/etc/shadowsocks.json`
- 重新启动：`/etc/init.d/shadowsocks restart`
- 加速：
	- 安装锐速(类似BBR)：`wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh`
	- TCP快速启动(TCP Fast Open)：`vim /etc/rc.local` 追加一条：`echo 3 > /proc/sys/net/ipv4/tcp_fastopen`；`vim /etc/sysctl.conf` 追加一条：`net.ipv4.tcp_fastopen = 3`；`vim /etc/shadowsocks.json`，修改一条：`"fast_open":true`

### shadowsocks.json说明

``` js

{
    "server":"0.0.0.0",
    "server_port": <ss服务端口>,
    "local_address":"127.0.0.1",
    "local_port": <ss本地端口>,
    "password":"<ss密码>",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":true
}

// 多密码支持
"port_password": {
	"<port>": "<password>"
}

```


## 相关文章

- [shadowsocks.org](shadowsocks.org)
- [shadowsocks_install](https://github.com/teddysun/shadowsocks_install)
- [shadowsocks libev 一键安装](https://github.com/iMeiji/shadowsocks_install/wiki/shadowsocks-libev-%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85)
- [serverspeeder-锐速破解版](https://github.com/91yun/serverspeeder)
- [开启TCP BBR拥塞控制算法](https://github.com/iMeiji/shadowsocks_install/wiki/%E5%BC%80%E5%90%AFTCP-BBR%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6%E7%AE%97%E6%B3%95) - 推荐，相较于serverspeeder而言，BBR更加快，且不用更改内核。