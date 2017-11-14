---
layout: post
title: 前端性能
date: 2017-11-09 20:00:00 +0800
categories:
- Tech
tags:
- 前端

---

## 前端性能思考

- **性能的衡量基于指标。**
- 性能需要阙值

## 前端衡量指标

指标：

- 白屏时间
- 首屏时间
- 用户可交互时间
- 完全加载时间
- 首字节时间
- DNS解析时间
- TCP连接时间
- HTTP请求时间
- HTTP响应时间
- PV
- UV
- QPS
- TPS

维度：

- 运营商
- 网络
- URL

### TP90、TP99

TP是指 Top Percentile，

- TP90是指在服务响应90%的网络请求中，一个最低的耗时。英文表述更加准确"***TP90 is a minimum time under which 90% of requests have been served.***" from [What do we mean by “top percentile” or TP based latency?](https://stackoverflow.com/questions/17435438/what-do-we-mean-by-top-percentile-or-tp-based-latency)

### 优化点

- 高频事件消抖、节流。使用`_.debounce()`和`_. throttle()`，控制高频事件的操作，如：`scroll`、`onChange`
	- `_.debounce()`的多次连续的调用，最终实际上只会调用一次；
	- `_. throttle()`将频繁调用的函数限定在一个给定的调用频率内。
- JavaScritp很快，但是DOM很慢，减少修改DOM。
	- DOM的渲染需要计算DOM+CSSOM，每次DOM和CSSOM的每次修改都会触发重绘；
		- 渲染过程：JavaScript -> Style -> Layoout -> Paint -> Composite
	- 避免`position: fixed;`布局，Z轴图层堆叠关系会改变，引起重绘；
	- 图层隔离：将那些会变动的元素提升至单独的图层，比如：动画、渐变；
	- 降低图层复杂度；
	- 避免线程阻塞；
	- 优化CSS；
	- 文件：引入方式、位置、文件合并、延迟加载；
	- 硬件加速：GPU加速渲染

## 前端性能监控

### 如何监控

- [7 天打造前端性能监控系统](http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/)

### 监控过程

- 多渠道收集上报(设备、用户、IP、页面位置、上报类型、内容详情等)
- 集中数据存储
- 数据可视化(多维度可视化：地理位置、运营商、手机系统等)


## 前端性能优化

### 基于各环节优化(主抓关键路径)

> 了解其过程，你才能做好优化。

- 加缓存
- 水平扩容
- 减少IO
- 传输压缩(Gzip)
- 机器升级
- 算法优化
- 微服务/SOA
- 异步化
- 加队列
- 网络优化(关键)
- 利用硬件加速(多核CPU、GPU)

### HTML优化

- 避免HTML重绘


----

## 相关文章

- [7 天打造前端性能监控系统](http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/)
- [前端性能优化相关](https://github.com/wy-ei/notebook/issues/34)
- [《高性能网站建设指南》笔记](https://github.com/wy-ei/notebook/issues/15)
