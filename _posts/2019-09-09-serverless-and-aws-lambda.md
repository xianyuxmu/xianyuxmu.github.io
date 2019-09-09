---
layout: post
title: 无服务器 & AWS Lambda
date: 2019-09-09 19:05:00 +0800
categories:
- Tech
tags:
- 月读
- 2019年09月

---

## 无服务器 & AWS Lambda

Amazon CTO Werner Vogels曾经在AWS re:Invent大会上提到: 如果把云计算理解成一个执行环境，那么，在这个环境里，函数（即业务逻辑的载体）+数据（即跟业务相关的输入与输出）就是应用的核心，有了Functions、Data、Event这三者，其它任何代码和框架，无非是整个应用的胶水和UI罢了。那么，最理想的情况就是用最少的时间写胶水，将更多的时间投入到核心应用的开发中，甚至，彻底实现整个软件栈的微服务化。  - *引用自：[带您玩转Lambda，轻松构建Serverless后台！ | 亚马逊AWS官方博客](https://aws.amazon.com/cn/blogs/china/lambda-serverless/)*


AWS Lambda 是一项计算服务，可使您无需预配置或管理服务器即可运行代码。AWS Lambda 只在需要时执行您的代码并自动缩放，从每天几个请求到每秒数千个请求。您只需按消耗的计算时间付费 – 代码未运行时不产生费用。借助 AWS Lambda，您几乎可以为任何类型的应用程序或后端服务运行代码，并且不必进行任何管理。AWS Lambda 在可用性高的计算基础设施上运行您的代码，执行计算资源的所有管理工作，其中包括服务器和操作系统维护、容量预置和自动扩展、代码监控和记录。 - *引用自：[什么是 AWS Lambda？ - AWS Lambda](https://docs.aws.amazon.com/zh_cn/lambda/latest/dg/welcome.html)*

简单讲，***AWS Lambda***是Serverless的一种实现。

#### 下面是AWS Lambda的一个例子

执行代码：

![执行代码](https://s3.cn-north-1.amazonaws.com.cn/images-bjs/0410-1.png)

执行速度：

![执行速度](https://s3.cn-north-1.amazonaws.com.cn/images-bjs/0410-9.png)


### 无服务器特点

- 无服务器管理：不需要配置服务器环境，直接可执行
- 按价值付费：按次收费，不产生固定的机器费用
- 灵活扩展：服务伸缩不需要自己处理，由Lambda自动调整
- 自动化的高可用性

参考：[无服务器计算_云应用部署-AWS云服务](https://aws.amazon.com/cn/serverless/)

### 函数计算应用场景

基于事件触发：

1. IoT应用：设备端通过函数计算来订阅天气信息和空气质量，设备和设备之间无依赖，执行过程中无需记录状态，获取到第三方数据即可返回。
2. WEB应用：某WEB网站在用户注册成功后，会发一封欢迎邮件，通过函数计算把邮件内容定制成模板，每次触发，每次执行都是幂等无状态。
3. 图片处理：基于OSS的事件触发，当用户上传的图片转入到某Bucket中后，自动触发函数岁图片进行可定制化处理
4. 音频转换文字处理：当用户通过语音来发出某些指令的时候，可以通过函数计算来触发阿里云的ET公开API获取到音频转换成文字的方式。

基于云产品场景的触发：

1. OSS（云存储）应用：
	* 图片处理：监控Bucket、Object事件（Put、Delete、Get、Head等）触发用户对函数运行，比如图片的转码、水印或者鉴黄后的图片分发到多个区域的Bucket等。
	* 日志处理：通过函数计算事件触发，把存放的不同区域的bucket下的日志文件进行汇总，分解，计算，并把计算结果通过回调函数通知Invoke程序。
2. API Gateway的应用：在调用API Gateway之后对服务进行一层处理

![](https://yqfile.alicdn.com/10ca854e4f49c8e4e4adac0ce8efb2fda7235665.jpeg)

引用自：[入门篇：函数计算的基本概念和通用场景概述-云栖社区-阿里云](https://yq.aliyun.com/articles/106379)
