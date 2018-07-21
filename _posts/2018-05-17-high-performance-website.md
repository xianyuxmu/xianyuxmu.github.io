---
layout: post
title: 构建高性能Web站点
date: 2018-05-17 20:07:00 +0800
categories:
- Tech
tags:
- 技术
- 2018年05月

---

> 目录来自： [构建高性能Web站点](https://book.douban.com/subject/3924175/)

## 第1章 绪论

- 1.1 等待的真相
- 1.2 瓶颈在哪里
- 1.3 增加带宽
- 1.4 减少网页中的HTTP请求
- 1.5 加快服务器脚本计算速度
- 1.6 使用动态内容缓存
- 1.7 使用数据缓存
- 1.8 将动态内容静态化
- 1.9 更换Web服务器软件
- 1.10 页面组件分离
- 1.11 合理部署服务器
- 1.12 使用负载均衡
- 1.13 优化数据库
- 1.14 考虑可扩展性
- 1.15 减少视觉等待

## 第2章 数据的网络传输

- 2.1 分层网络模型
- 2.2 带宽
- 2.3 响应时间
- 2.4 互联互通

## 第3章 服务器并发处理能力

- 3.1 吞吐率
- 3.2 CPU并发计算
- 3.3 系统调用
- 3.4 内存分配
- 3.5 持久连接
- 3.6 I/O模型
- 3.7 服务器并发策略

## 第4章 动态内容缓存

- 4.1 重复的开销
- 4.2 缓存与速度
- 4.3 页面缓存
- 4.4 局部无缓存
- 4.5 静态化内容

## 第5章 动态脚本加速

- 5.1 opcode缓存
- 5.2 解释器扩展模块
- 5.3 脚本跟踪与分析

## 第6章 浏览器缓存

- 6.1 别忘了浏览器
- 6.2 缓存协商
- 6.3 彻底消灭请求

## 第7章 Web服务器缓存

- 7.1 URL映射
- 7.2 缓存响应内容
- 7.3 缓存文件描述符

## 第8章 反向代理缓存

- 8.1 传统代理
- 8.2 何为反向
- 8.3 在反向代理上创建缓存
- 8.4 小心穿过代理
- 8.5 流量分配

## 第9章 Web组件分离

- 9.1 备受争议的分离
- 9.2 因材施教
- 9.3 拥有不同的域名
- 9.4 浏览器并发数
- 9.5 发挥各自的潜力

## 第10章 分布式缓存

- 10.1 数据库的前端缓存区
- 10.2 使用memcached
- 10.3 读操作缓存
- 10.4 写操作缓存
- 10.5 监控状态
- 10.6 缓存扩展

## 第11章 数据库性能优化

- 11.1 友好的状态报告
- 11.2 正确使用索引
- 11.3 锁定与等待
- 11.4 事务性表的性能
- 11.5 使用查询缓存
- 11.6 临时表
- 11.7 线程池
- 11.8 反范式化设计
- 11.9 放弃关系型数据库

## 第12章 Web负载均衡

- 12.1 一些思考
- 12.2 HTTP重定向
- 12.3 DNS负载均衡
- 12.4 反向代理负载均衡
- 12.5 IP负载均衡
- 12.6 直接路由
- 12.7 IP隧道
- 12.8 考虑可用性

## 第13章 共享文件系统

- 13.1 网络共享
- 13.2 NFS
- 13.3 局限性

## 第14章 内容分发和同步

- 14.1 复制
- 14.2 SSH
- 14.3 WebDAV
- 14.4 rsync
- 14.5 Hash tree
- 14.6 分发还是同步
- 14.7 反向代理

## 第15章 分布式文件系统

- 15.1 文件系统
- 15.2 存储节点和追踪器
- 15.3 MogileFS

## 第16章 数据库扩展

- 16.1 复制和分离
- 16.2 垂直分区
- 16.3 水平分区

## 第17章 分布式计算

- 17.1 异步计算
- 17.2 并行计算

## 第18章 性能监控

- 18.1 实时监控
- 18.2 监控代理
- 18.3 系统监控
- 18.4 服务监控
