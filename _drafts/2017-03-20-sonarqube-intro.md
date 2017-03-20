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

由于研发人员能力习惯的差异、历史代码不断堆积

## 如何提高代码质量

- 研发规范；
- 代码 Review；
- 代码 lint 工具；
- 研发人员意识、工程素质的提高（心要到达）；
- 加入测试（单元测试、回归测试）；
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

SonarQube 特性：

- 即时跟踪项目质量变化；
- 检查代码的复杂度；
- 指出重复代码；
- 统计单元测试覆盖率；
- 发现 Bug（Code Smells、Security Vulnerability、自定义规则）；
- 集成所有项目（风险视图、总体质量报告）；


## 相关资料

- [度量和提高代码质量](http://www.infoq.com/cn/news/2016/01/measure-improve-code-quality)
- [Quora：代码质量和发展速度可以兼得](http://www.infoq.com/cn/news/2015/08/quora-qlint?utm_source=news_about_Code_Quality&utm_medium=link&utm_campaign=Code_Quality)
- [Google之类公司的代码质量如何？](http://blog.jobbole.com/74107/)
- [QA的未来](http://www.infoq.com/cn/news/2016/11/future-qa-atlassian?utm_source=news_about_Code_Quality&utm_medium=link&utm_campaign=Code_Quality)
- [使用 Sonar 进行代码质量管理](https://www.ibm.com/developerworks/cn/java/j-lo-sonar/)
- [SonarQube - Continuous Code Quality](https://www.sonarqube.org/) 

----

Robin on March 20, 2017 Monday