---
layout: post
title: Git 使用规范
date: 2017-06-23 20:38:00 +0800
categories:
- Tech
tags:
- Git

---

## 分支

一般来说，一个项目有两个主要的分支，开发分支和生产分支。开发分支用于迭代开发，生产分支用于项目版本的发布。

### 分支策略

在大型软件的开发过程中，通常有很多人参与，需要对分支进行管理。下Git分支管理可以使用[Introducing GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html)、[Github flow](http://scottchacon.com/2011/08/31/github-flow.html)(更多分支管理策略介绍，请查看：[Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html))。

- `master`，主干分支通常只有一个，`master`分支与`release`分支保持一致，通常在`master`分支上打tag，用于标记软件版本
- `release`，发布分支通常只有一个，是项目的开发的主线，用于项目的发布任务和长期维护。`develop`分支在每个迭代完成之后，合并到`release`。
- `develop`，开发分支，是开发的主要分支。
- `feature`，特性分支的命名以`feature/`开头，如：`feature/new-page`。一个开发迭代中可能有多个`feature`，多个`feature`各自完成后合并到`develop`分支。
- `hotfix`，紧急修复分支的命名以`hotfix/`开头，如：`hotfix/api-error`。通常从生产分支(通常为`master`)新建产生。
- `bugfix`，问题修复分支的命名以`bugfix/`开头，如：`bugfix/username-not-display`。通常从`release`分支新建产生，`bugfix`分支与`hotfix`分支的差异是，`bugfix`分支可以从各种分支中新建产生并用于修复问题，而`hotfix`一般只从`生产分支`新建产生。

![Git Flow](https://datasift.github.io/gitflow/GitFlowHotfixBranch.png)

Git Flow, from [Introducing GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html)

## Git 使用规范流程

请查看[《Git 使用规范流程》](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)。

----


### 关于合并提交

只允许本地开发的时候进行“合并提交”操作，任何提交到远程的分支尽量不要进行合并提交操作。

- 合并提交的必要性？
	- 为了维护主干分支(`master`)的简洁，`master`中的每个人提交记录要简洁。
	- 在开发分支中，往往的存在一个改动点多个提交的情况(一个改动点，进行多次bugfix等)，这些有必要进行合并。
- 什么时候合并提交？
	- 多个连续的提交可以合成一个提交时，如：多个提交为一个改动点，但是分为多次提交。

----

Robin on 21 June, 2017
