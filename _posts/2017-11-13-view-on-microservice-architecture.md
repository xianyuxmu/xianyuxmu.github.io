---
layout: post
title: 微服务架构思考
date: 2017-11-13 20:00:00 +0800
categories:
- Tech
tags:
- 微服务

---

![Monoliths and Microservices](https://martinfowler.com/articles/microservices/images/sketch.png)

Monoliths and Microservices, from [*Microservices*](https://martinfowler.com/articles/microservices.html)

## 什么是微服务架构(Microservice Architecture)?

- 微服务架构就像搭积木，每个微服务都是一个零件，并使用这些零件组装出不同的形状。
- 微服务架构就是把一个大系统按业务功能分解成多个职责单一的小系统，并利用简单的方法使多个小系统相互协作，组合成一个大系统。
- 微服务架构就是把因相同原因而变化的功能聚合到 一起，而把因不同原因而变化的功能分离开，并利用轻量化机制(通常为 HTTP RESTful API)实现通信。
- 微服务架构是一种架构模式，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调、互相配合，为用户提供最终价值。每个服务运行在其独立的进程中，服务与服务间采用轻量级的通信机制互相协作(通 常是基于 HTTP 协议的 RESTful API)。每个服务都围绕着具体业务进行 构建，并且能够被独立的部署到生产环境、类生产环境等。另外，对具体 的服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建。 *- 《微服务架构与实践》，王磊*
- 微服务架构 ≈ 模块化开 发 + 分布式计算。
- 核心思想: 把大系统拆分成小型系统，把大事化小，以降低系统的复杂性，从 而大幅降低系统建设、升级、运维的风险和成本。
- 所有团队的模块都要以 Service Interface 的方式将数据和功能开放出来。 *- Jeff  Bezos*
- “微服务”与“微服务架构”是有本质区别的。“微服务”强 调的是服务的大小，它关注的是某一个点。而“微服务架构”则是一种架构 思想，需要从整体上对软件系统进行通盘的考虑。
- **A definition of this new architectural term:** The term "***Microservice Architecture***" has sprung up over the last few years to describe a particular way of designing software applications as suites of ***independently deployable services***. While there is no precise definition of this architectural style, there are *certain common characteristics around organization around business capability*, automated deployment, intelligence in the endpoints, and decentralized control of languages and data. - *from [Microservices](https://martinfowler.com/articles/microservices.html), by Martin Fowler.*

----

## 思考微服务架构

- Docker的诞生，微服务概念及架构的推广和落地变得更加的可靠和方便。
- 未来的软件，是基于微服务进行组装的，不需要从头开始开发。
- 微服务关键词：注册中心、服务发现、负载均衡、服务网关、管理端集成框架、持续集成、配置中心、监控告警
- 单体应用被微服务化后，一个单体应用被拆成了很多个微服务，系统 的健康巡检、性能监控、业务指标健康、文件备份监控、数据库备份监控、定时任务执行情况监控都变得困难。
- 使用HTTP API比RPC更加灵活。有关资料：**算上序列化的时间，RPC协议的吞吐量是HTTP性能 两倍。**例如：进行 10000 次连续请求的测试结果，总耗时 1.504 秒，平均每个请求耗时 0.15 毫秒。
- 微服务架构下的数据一致性：
	- ACID
	- CAP，是指在一个分布式系统下，包含三个要素:Consistency(一致性)、 Availability(可用性)、Partition tolerance(分区容错性)，并且三者不可得兼。
	- BASE。BA:Basically Available，基本可用；S:Soft State，软状态，状态可以有一段时间不同步；E:Eventually Consistent，最终一致。
- “Microservices came to be because of containers” is a myth. We were already doing microservices on fucking Weblogic!

----

以上内容摘自 [Re：从 0 开始的微服务架构](http://www.infoq.com/cn/minibooks/microservice--from-zero)

## 相关文章

- [SOA和微服务架构的区别？](https://www.zhihu.com/question/37808426)