---
layout: post
title: 网络协议
date: 2017-09-30 20:19:00 +0800
categories:
- Tech
tags:
- TCP/IP
- HTTP

---



## 顶层思考

- 之所以这样设计是因为：
	- 当时遇到了什么问题，在当时的场景下，解决了当时的问题。
	- 够用了。
	- 不这样设计也是可以的。
- 每一层协议是为了解决上一层的问题，并增加新的能力。最终确保应用层传输的可靠。

----

## TCP/IP协议集

![](https://technet.microsoft.com/en-us/library/bb726993.caop0201_big(l=en-us).gif)

from [Chapter 2 – Architectural Overview of the TCP/IP Protocol Suite](https://technet.microsoft.com/en-us/library/bb726993.aspx)


![TCP/IP Protocol](http://static.thegeekstuff.com/wp-content/uploads/2011/10/tcp-ip.png)

TCP/IP Protocol


![TCP Transfer Process](http://www.tenouk.com/Module42_files/image014.png)

[LINUX SOCKET PART 15 Advanced TCP/IP - The TCP/IP Protocols & RAW Socket](http://www.tenouk.com/Module42a.html)

![七层网络和四层协议](https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_OverTCPIP.png)

七层网络和四层协议

![OSI Model & TCP/IP](http://img.tfd.com/cde/TCPIP.GIF)

OSI Model & TCP/IP

**相关资料：**

- [OSI model](https://en.wikipedia.org/wiki/OSI_model)
	- fullname: ***Open Systems Interconnection model***
	- Including protocol data unit & functions.
- Why does the OSI have 7 layers?
	- The International Standards Organization (ISO) developed the Open Systems Interconnection (OSI) model. It divides network communication into seven layers. Layers 1-4 are considered the lower layers, and mostly concern themselves with moving data around. Layers 5-7, the upper layers, contain application-level data.
	- ***The OSI model as you correctly mentioned, does have 7 layers and the reason is simple… The ISO decided that 7 layers was adequate to create the reference model they wanted!*** If the OSI model had more or less layers, it wouldn't mean that the protocols or software created would have extra or less functionality of what they have today, because as we already said, **this is a reference model**.

----

## 四层网络

### 应用层

- 超文本传输协议(HTTP):万维网的基本协议.
- 文件传输(TFTP简单文件传输协议).
- 远程登录(Telnet),提供远程访问其它主机功能,它允许用户登录 internet主机,并在这台主机上执行命令.
- 网络管理(SNMP简单网络管理协议),该协议提供了监控网络设备的方法,以及配置管理,统计信息收集,性能管理及安全管理等.
- 域名系统(DNS),该系统用于在internet中将域名及其公共广播的网络节点转换成IP地址.

### 运输层

- TCP 传输控制协议
- UDP 用户数据报协议

### 网络层

处理分组在网络中的活动，如分组的选路；网络层的协议包括:

- IP协议
- ICMP协议（Internet互联网控制报文协议）
- IGMP协议（Internet组管理协议）

### 链路层

也称数据链路层或网络接口层，包括设备驱动程序和网络接口卡，它们一起处理与电缆的物理接口细节。链路层主要有三个目的:

- 为IP模块发送和接受IP数据
- 为ARP模块发送ARP请求和接受ARP应答
- 外RARP发送RARP请求和接受RARP应答

----

## 七层协议

### 第7层 应用层(Application Layer)

提供为应用软件而设的界面，以设置与另一应用软件之间的通信。例如: HTTP，HTTPS，FTP，TELNET，SSH，SMTP，POP3等。

### 第6层 表达层(Presentation Layer)

把数据转换为能与接收者的系统格式兼容并适合传输的格式。

### 第5层 会话层(Session Layer)

负责在数据传输中设置和维护电脑网络中两台电脑之间的通信连接。

### 第4层 传输层(Transport Layer)

把传输表头（TH）加至数据以形成数据包。传输表头包含了所使用的协议等发送信息。例如:传输控制协议义（TCP）等。

### 第3层 网络层(Network Layer)

决定数据的路径选择和转寄，将网络表头（NH）加至数据包，以形成分组。网络表头包含了网络数据。例如:互联网协议（IP）等。

### 第2层 数据链接层(Data Link Layer)

负责网络寻址、错误侦测和改错。当表头和表尾被加至数据包时，会形成了帧。数据链表头（DLH）是包含了物理地址和错误侦测及改错的方法。数据链表尾（DLT）是一串指示数据包末端的字符串。例如以太网、无线局域网（Wi-Fi）和通用分组无线服务（GPRS）等。
分为两种子层：logic link control sublayer & media access control sublayer

### 第1层 物理层(Physical Layer)

在局部局域网上传送帧，它负责管理电脑通信设备和网络媒体之间的互通。包括了针脚、电压、线缆规范、集线器、中继器、网卡、主机适配器等

----

## HTTP

超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础。

设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。通过HTTP或者HTTPS协议请求的资源由统一资源标识符（Uniform Resource Identifiers，URI）来标识。

from [超文本传输协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)

### 请求报文和响应报文

一个HTTP请求报文由请求行（request line）、请求头部（header）、空行和请求数据4个部分组成。

客户端请求：

```
GET / HTTP/1.1
Host: www.google.com

```

服务器应答：

```
HTTP/1.1 200 OK
Content-Length: 3059
Server: GWS/2.0
Date: Sat, 11 Jan 2003 02:44:04 GMT
Content-Type: text/html
Cache-control: private
Set-Cookie: PREF=ID=73d4aef52e57bae9:TM=1042253044:LM=1042253044:S=SMCc_HRPCQiqy
X9j; expires=Sun, 17-Jan-2038 19:14:07 GMT; path=/; domain=.google.com
Connection: keep-alive

```

`Connection: keep-alive` 保持连接，避免重复TCP初始握手活动，减少网络负荷和响应周期。

### HTTP/2

HTTP/2（超文本传输协议第2版，最初命名为HTTP 2.0），简称为h2（基于TLS/1.2或以上版本的加密连接）或h2c（非加密连接），是HTTP协议的的第二个主要版本，使用于万维网。

HTTP/2是HTTP协议自1999年HTTP 1.1发布后的首个更新，主要基于SPDY协议。

通过以下举措，减少 网络延迟，提高浏览器的页面加载速度：

- 对HTTP头字段进行数据压缩(即HPACK算法)；
- HTTP/2 服务端推送(Server Push)；
- 请求管线化；
- 修复HTTP/1.0版本以来未修复的 队头阻塞 问题；
- 对数据传输采用多路复用，让多个请求合并在同一 TCP 连接内。

**HTTP/2与HTTP/1.1比较:**

- HTTP/2 相比 HTTP/1.1 的修改并不会破坏现有程序的工作，但是新的程序可以借由新特性得到更好的速度。
- HTTP/2 保留了 HTTP/1.1 的大部分语义，例如请求方法、状态码乃至URI和绝大多数HTTP头部字段一致。而 HTTP/2 采用了新的方法来编码、传输客户端——服务器间的数据。

----

Robin on No.4, East Road, Wangjing