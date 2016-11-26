---
layout: post
title: "CentOS 7 安装桌面和远程连接支持"
date: 2016-10-15 14:31:19 +0800
categories:
- Technology
tags:
- GNOME
- CentOS
---

**ECS 配置：**CPU(1 核)、内存(1024MB + 1024MB Swap)、带宽(4Mbps Max)。

**结论：**安装完之后可以连接，但是实际的使用效果不理想。ECS CPU 消耗大、低带宽下卡顿、会发生卡死。主机如果作为服务器使用，没必要安装桌面，控制台是最好的交互。

<!-- more -->

[How To Install and Configure VNC Remote Access for the GNOME Desktop on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-remote-access-for-the-gnome-desktop-on-centos-7)

[CentOS 7 how to stop / start Gnome desktop from command line [closed]](http://stackoverflow.com/questions/39012285/centos-7-how-to-stop-start-gnome-desktop-from-command-line)

第一步，安装桌面（`GNOME-Desktop`）：

```
# yum -y groups install "GNOME Desktop" 
```
安装完成后，输入：

```
# startx 
```

如果命令存在，代表安装成功！

----

第二步，安装VNC（VNC Server）：

[VNC-Server installation on CentOS 7](https://www.howtoforge.com/vnc-server-installation-on-centos-7)

启动：vncserver

vncserver -geometry 800x600

查看 VNC 监听的接口：

```	
# sudo lsof -i -P | grep -i "listen"

```

```
[root@iZ941u4kh5rZ ~]# sudo lsof -i -P | grep -i "listen"
cupsd      920   root   12u  IPv4  15441      0t0  TCP localhost:631 (LISTEN)
sshd       922   root    3u  IPv4  15395      0t0  TCP *:22 (LISTEN)
dnsmasq   1084 nobody    6u  IPv4  15977      0t0  TCP iZ941u4kh5rZ:53 (LISTEN)
Xvnc      2009   root    8u  IPv4  19126      0t0  TCP *:5901 (LISTEN)
Xvnc      2009   root    9u  IPv6  19127      0t0  TCP *:5901 (LISTEN)
Xvnc      8252   root    8u  IPv4  62093      0t0  TCP *:5902 (LISTEN)
Xvnc      8252   root    9u  IPv6  62094      0t0  TCP *:5902 (LISTEN)
```


Mac 连接 VNC

打开Finder，点击左上角的 “前往” 选择 “连接服务器”。

输入服务器地址：vnc:// ip:端口

点击连接即可！

end







