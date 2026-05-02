---
layout: post
title: 技术专题文章
date: 2018-04-15 13:38:00 +0800
categories:
- Tech
tags:
- 技术专题

---


### Nginx

- [Nginx 容器教程](http://www.ruanyifeng.com/blog/2018/02/nginx-docker.html)


### Docker

- [Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)


### HTTP/HTTPS

- [互联网协议入门（一）](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
- [DNS 原理入门](http://www.ruanyifeng.com/blog/2016/06/dns.html)
	- 你可以把它想象成一本巨大的电话本。
- [HTTP/2 服务器推送（Server Push）教程](http://www.ruanyifeng.com/blog/2018/03/http2_server_push.html)

### Node.js

- [如何评价阿里开源的企业级 Node.js 框架 EggJS？](https://www.zhihu.com/question/50526101/answer/144952130)


### 泛前端

- [你为什么不使用 TypeScript？](https://www.zhihu.com/question/273619114)

### 云计算

- [阿里云的各种产品都是干什么的？](https://www.zhihu.com/question/24795126/answer/41691845)
- 阿里云中关于kubernetes的文章
	- [【DockerCon2017最新技术解读】如何在阿里云一键部署高可用的Kubernetes集群](https://yq.aliyun.com/articles/91379)
	- [使用 kubeadm 创建一个 kubernetes 集群](https://yq.aliyun.com/articles/431059)
	- [当 Kubernetes 遇到阿里云](https://yq.aliyun.com/articles/68921)
	- [阿里云上Kubernetes集群联邦](https://yq.aliyun.com/articles/401059)
- [详解SLB、EIP、NAT网关之间区别， 合理选择云上公网入口](https://yq.aliyun.com/articles/391631)
	- 负载均衡SLB: 对多台云服务器进行流量分发的负载均衡服务，可以通过流量分发扩展应用系统对外的服务能力，通过消除单点故障提升应用系统的可用性。
		- 注意：负载均衡SLB仅提供被动访问公网的能力，即后端ECS只能在收到通过负载均衡SLB转发来的公网的请求时，才能访问公网回应该请求，不具备SNAT功能。
	- 弹性公网IP（EIP）: 独立的公网IP资源，可以绑定到阿里云专有网络VPC类型的ECS、NAT网关、私网负载均衡SLB上，并可以动态解绑，实现公网IP和ECS、NAT网关、SLB的解耦，满足灵活管理的要求。
		- 注意：EIP只能绑定一台ECS，同时提供访问公网和被公网访问的能力。
	- NAT网关: 帮助您在VPC环境下构建一个公网流量的出入口，通过自定义SNAT，DNAT规则灵活使用网络资源，支持多IP，支持共享公网带宽。


### Kubernetes

- [cantbewong/Install-Kubernetes-baremetal.md](https://gist.github.com/cantbewong/476efcc1f0a8be9f7b83a4e043b9fd68)
- Kubernetes运行配置: `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`