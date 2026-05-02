---
layout: post
title: SonarQube——持续代码质量管理
date: 2017-03-20 20:11:00 +0800
categories:
- Tech
tags:
- SonarQube

---

# 简介

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

> Contents above are written by Robin on March 20, 2017 Monday

# 本地 SonarQube

## 安装

> 以 Mac 为例。

教程: [Get Started in Two Minutes](https://docs.sonarqube.org/display/SONAR/Get+Started+in+Two+Minutes)

### 基本安装

1. 下载 [SonarQube](http://www.sonarsource.org/downloads/)，并解压到 `/etc/sonarqube`
2. 下载 [SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner)，并解压到 `/etc/sonar-scanner`
3. 执行 `/etc/sonarqube/bin/[OS]/sonar.sh console` 命令启动 SonarQube（注意要把 `[OS]` 替换成 `/etc/sonarqube/bin/` 下存在的某个文件路径。）。在浏览器中打开 [http://localhost:9000](http://localhost:9000)，就可以访问本地的 `SonarQube`。
4. 开始扫描代码。切换到项目路径 `/your-project-name`，执行 `/etc/sonarqube/bin/sonar-scanner -Dsonar.projectKey=your-project-name -Dsonar.sources=.`
5. 代码扫描完成后，打开 [http://localhost:9000](http://localhost:9000)，然后登陆(默认用户名: admin，密码: admin)查看扫描报告。

如果出现下面这个问题：

```
INFO: ------------------------------------------------------------------------
INFO: EXECUTION FAILURE
INFO: ------------------------------------------------------------------------
Total time: 7.572s
Final Memory: 8M/223M
INFO: ------------------------------------------------------------------------
ERROR: Error during Sonar runner execution
ERROR: Unable to execute Sonar
ERROR: Caused by: You must define the following mandatory properties for 'Unknown':   sonar.projectKey, sonar.projectName, sonar.projectVersion, sonar.sources
ERROR:
ERROR: To see the full stack trace of the errors, re-run SonarQube Runner with the -e switch.
ERROR: Re-run SonarQube Runner using the -X switch to enable full debug logging.
```

> 常见的一个错误，解决方案在这里：[Sonar Setup Undefined Mandatory Properties](http://stackoverflow.com/questions/21204350/sonar-setup-undefined-mandatory-properties)。

你需要在待运行 SonarQube 检查的项目目录下创建一个文件：`sonar-project.properties`，然后增加以下内容：

```
# Required metadata／必要字段
sonar.projectKey=sonar-runner-simple
sonar.projectName=项目名称(这个名称将在 SonarQube 中显示)
sonar.projectVersion=1.0

# 设置为当前路径
sonar.sources=.

# 设置待检查项目的语言类型
sonar.language=js

# Encoding of the source files
sonar.sourceEncoding=UTF-8
```


### 使用 sonarlint 集成到开发环境(IDE)

使用 [SonarLint](http://www.sonarlint.org/index.html) 插件，可以把 SonarQube 集成到本地开发环境中，如：Eclipse、IntelliJ IDEA、WebStorm 以及 Visual Studio 等 IDE 中。

继承步骤（以 `IntelliJ IDEA` 为例子）：

- 下载 [SonarLint for IntelliJ IDEA](http://www.sonarlint.org/intellij/index.html)(链接中包含教程)，下载完成后不用解压文件。
- 打开 `IntelliJ IDEA` > Preferences > Pulgin，点击 `Install Pulgin from disk...` ，然后选中刚才下载的文件并安装。
- 接着在 `IntelliJ IDEA` > Preferences > Other Settings 中选中 `SonarLint General Settings`，填写：
	- Nmae：起个名字
	- Server URL：`http://localhost:9000/`
	- Token: (这个值需要点击 `Create Token`，然后在浏览器打开的页面中生成一个 `Token`，然后复制到这里)
- 点击 `Test connection`，成功后点击 `OK` 退出弹框。
- 然后选择 `SonarLint Project Settings`，点击选中 `Enable binding to remote SonarQube server`，然后：
	- 在 `Bind to Server` 选择我们刚才在 `SonarLint General Settings` 添加的本地服务器。
- 完成本地 IDE 与本地 SonarQube 服务接入。

----

Robin on April 18, 2017 Tuesday