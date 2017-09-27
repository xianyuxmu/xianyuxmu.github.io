---
layout: post
title: September 2017 月读
date: 2017-09-02 20:19:00 +0800
categories:
- Reading
- Life
tags:
- 月读

---

<blockquote class="blockquote-center">
<p>Are You Missing Purpose? </p>
</blockquote>

## TOP

- [22zweizwei](https://www.youtube.com/user/22zweizwei/feed)
	- [New York in 4K](https://www.youtube.com/watch?v=TmDKbUrSYxQ)


## Life

- [Innovation Hack: The Law of Rich Input](https://medium.com/the-mission/innovation-hack-the-law-of-rich-input-d43d8f4e1911)
	- “Garbage in, Garbage out” is a maxim that comes from the world of computing.
	- “If you put wrong figures into the machine, will the right answers come out?”
- [How to Find Your Mission and Success](https://medium.com/the-mission/how-to-find-your-mission-and-success-f24b9cd69fcb)
- [卡辛斯基的警告](http://survivor.ruanyifeng.com/future/unabomber.html)
- [你的B计划在哪里？](http://survivor.ruanyifeng.com/plan-b/plan-b.html)
- [为什么上班和下班已经过时](http://survivor.ruanyifeng.com/collapse/remote-work.html)

----

## Future




----

## Money

- [风险无法通过模式来解决](https://zhuanlan.zhihu.com/p/28683047)
	- 风险是无法通过模式来解决，只能通过相对高资金利息来弥补。

----

## Business




----

## 产品



----

## Tech

- [Understand your Distributed Application with Elasticsearch, Logstash and Kibana by *Benoît Quartier, Camptocamp*](https://portal.klewel.com/watch/webcast/elasticsearch-meetup-november-2016/talk/1)
- [滴滴是如何高效率处理线上故障的？](http://www.infoq.com/cn/news/2017/08/didi-ops-practise)
- [随行付孙志坤：DevOps企业落地，工具比脚本更重要](https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247485633&idx=1&sn=ce24f039077b6baf6e6e2fd6ba3e7adb)
- [JavaScript正在吞噬这个世界](http://www.infoq.com/cn/news/2017/08/JavaScript-eating-world)
- **单点故障**（single point of failure）简称SPOF，，是指系统中一旦失效，就会让整个系统无法运作的部件[1]，换句话说，是单点故障、全体故障。
	- 最重要的是，每个域至少要有2个域控制器。
	- 域控制器不要放在同一个物理位置。
	- 部属多个AD依赖的系统。
- [Fork A Repo](https://help.github.com/articles/fork-a-repo/)
	- **A fork is a copy of a repository.** Forking a repository allows you to freely experiment with changes without affecting the original project.
- [阿里云容器服务持续交付](https://github.com/AliyunContainerService/DevOps)
- 而对于DevOps只有三个问题是严肃的：
	- 如何重建你的系统（How to recreate your system？）
	- 如何安全地部署你的系统（How to safely change your system）
	- 部署后的问题监控与解决（When something has gone wrong)
- [基于容器服务的持续集成与云端交付（一）- 交付之禅](http://www.infoq.com/cn/articles/CICDInCaaS-DeliveryPrinciple)
- [DevOps与阿里云容器服务（三）](https://yq.aliyun.com/articles/58414)
	- 灰度发布
	- A/B Test
- Jenkins 2.0实现持续集成
	- [使用阿里云容器服务Jenkins 2.0实现持续集成之the tag you want篇(updated on 2017.09.06)](https://yq.aliyun.com/articles/72703)
	- [使用阿里云容器服务Jenkins实现持续集成和Docker镜像构建(updated on 2017.3.3)](https://yq.aliyun.com/articles/53971)
	- [使用阿里云容器服务Jenkins 2.0实现持续集成之Pipeline篇(updated on 2016.12.23)](https://yq.aliyun.com/articles/64970)
	- [容器服务slack运维机器人](https://yq.aliyun.com/articles/58422)
- [Merging vs. Rebasing - Git](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
	- This creates a new “merge commit” in the feature branch that ties together the histories of both branches.
	- But, instead of using a merge commit, rebasing re-writes the project history by creating brand new commits for each commit in the original branch.
- [弹性伸缩部署](https://yq.aliyun.com/articles/4226)
	- 弹性伸缩的的基础和关键在资源调度，分为自动伸缩、计划伸缩：
	- 自动伸缩：根据日常负载情况，计算应用所需的计算资源，以此达到降低成本、提升稳定性的目的。
	- 计划伸缩：也称作定时伸缩，应用场景主要是根据计划提前做好各个系统的容量准备，以便承受可预见的瞬间访问高峰。
	- 弹性伸缩在业界有两个方向，一个是垂直化的扩展（Scale up），一个水平化的扩展（Scale out）。从业务发展的角度来看应该是水平扩展的能力，这要求业务都是无状态的，通过负载均衡技术将访问请求分配到集群每一台机器上，不管是增加还是减少机器，业务的连续性都不应受到影响。
- [Apache Kafka™ is a distributed streaming platform. What exactly does that mean?](https://kafka.apache.org/intro.html)
- [AWS 案例研究：猎豹移动](https://aws.amazon.com/cn/solutions/case-studies/cheetah-mobile/?hp=tile)
- [Git LFS](https://www.atlassian.com/git/tutorials/git-lfs)
- [Reddit 是如何面试工程师的](https://mp.weixin.qq.com/s?__biz=MzIwMzg1ODcwMw==&mid=2247486804&idx=1&sn=0f8a9e8e0ef78ee3c817100921286bdf)
- [Mesos Mr-Redis 沙箱试用到上线](http://dockone.io/article/2658)
	- [Introduction to Redis Cluster](http://intro2libsys.com/focused-redis-topics/day-one/intro-redis-cluster)
- [DevOps实践-打造自服务持续交付 -下](http://www.infoq.com/cn/articles/devops--build-self-service-continuous-delivery-part02)
- [小红书里的秘密：机器学习如何帮助十人算法团队快速达成目标](https://mp.weixin.qq.com/s?__biz=MzA5NzkxMzg1Nw==&mid=2653162991&idx=1&sn=0340dab728514c9998f6af10aab4d412)
- [React的替代方案Inferno发布1.0版本](http://www.infoq.com/cn/news/2017/01/inferno-1-react-javascript)
- [Classes (ES6) Sample](https://googlechrome.github.io/samples/classes-es6/)
- [React Binding Patterns: 5 Approaches for Handling this](https://medium.freecodecamp.org/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56)
- HTML标签
	- [HTML Element Reference](https://www.w3schools.com/TAgs/ref_byfunc.asp)
	- [HTML element reference - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)


----

## 思考

- 今天参加“江南愤青”的分享，观点不错。
- 人要学会如何更好地与优秀的人一块共事，不要浪费时间适应、改造笨拙的人。
- 第三季度即将结束，将迎来25岁前最后的三个月，make it meaningful. *- 21 Sep. 2017*
- I need A PLAN! *- 23 Sep. 2017*
- 今天买了几盒月饼，寄给朋友。 *- 24 Sep. 2017*
- 寄出了月饼。 *- 25 Sep. 2017*
