---
layout: post
title: SonarQube——持续代码质量管理
date: 2017-03-20 20:11:00 +0800
categories:
- Tech
tags:
- SonarQube

---

## 代码质量重要性

代码质量的重要性是不言而喻的，不用过多的强调。

## 什么导致了代码质量下降

- 持续不断的代码变更；
- 持续不断的架构演变；
- 参差不齐的人员素质以及习惯；

## 如何提高代码质量

- 良好的研发规范；
- 代码 Review；
- 代码 lint 工具；
- 研发人员意识、工程素质的提高（心要到达）；
- 加入测试（单元测试、回归测试、覆盖率测试）；
- 持续追求代码质量，不断改进 CQA 方案(CQA: Code Quality Analysis)；


## 禅意代码质量

- 重复的可以合并
- 零散的可以集中
- 复杂的可以拆分
- 没用的可以删除
- 完美并不存在
- 复制的代码记得修改
- 不要依赖手工检查
- 定时清查代码，减少变动成本

## SonarQube 介绍

> [Sonar](https://www.sonarqube.org/) 是一个用于代码质量管理的开放平台。通过插件机制，Sonar 可以集成不同的测试工具，代码分析工具，以及持续集成工具，Sonar 可以方便地被集成到 Jenkins、Travis CI。 -- *IBM*

Sonar 免费支持 Java、JavaScript、Python、PHP、C#、XML、CSS、Android (Android lint) ，而收费的第三方插件

SonarQube 特性：

- 即时跟踪项目质量变化；
- 复杂度分析；
- 指出重复代码；
- 测试和覆盖率；
- 代码风格（Code Smells、Security Vulnerability、自定义规则）；
- 集成所有项目（风险视图、总体质量报告）；


## 相关资料

- [度量和提高代码质量](http://www.infoq.com/cn/news/2016/01/measure-improve-code-quality)
- [Quora：代码质量和发展速度可以兼得](http://www.infoq.com/cn/news/2015/08/quora-qlint?utm_source=news_about_Code_Quality&utm_medium=link&utm_campaign=Code_Quality)
- [Google之类公司的代码质量如何？](http://blog.jobbole.com/74107/)
- [QA的未来](http://www.infoq.com/cn/news/2016/11/future-qa-atlassian?utm_source=news_about_Code_Quality&utm_medium=link&utm_campaign=Code_Quality)
- [Quality assistance: how Atlassian does QA](https://www.atlassian.com/inside-atlassian/qa)
- [使用 Sonar 进行代码质量管理](https://www.ibm.com/developerworks/cn/java/j-lo-sonar/)
- [SonarQube - Continuous Code Quality](https://www.sonarqube.org/) 

----

Robin on March 20, 2017 Monday