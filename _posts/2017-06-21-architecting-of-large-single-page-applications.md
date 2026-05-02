---
layout: post
title: 大型SPA应用的设计
date: 2017-06-21 20:00:00 +0800
categories:
- Tech
tags:
- JavaScript
- Single Page Applications

---

![React Redux](https://onsen.io/blog/content/images/2016/Jun/react_redux.png)

- 本质上，大型应用的核心是管理数据，以及数据相关的状态(states)；
- 使用对象进行传递数据，实现函数参数可扩展；


## 关于分模块开发的大型 SPA 的部署过程思考

1. 部署目标原则：Simple、Rollbackable、Reliable。
2. 尽量降低某块之间的开发、部署过程的耦合度，尽量使用分模块开发、分模块部署。
3. 尽量避免全量(所有)或者禁止全量部署，防止全量部署可能带来的灾难性后果。

## 相关文章

- [Scalable Single-Page Application Architecture](http://blog.mgechev.com/2016/04/10/scalable-javascript-single-page-app-angular2-application-architecture/)

	
