---
layout: post
title: 软件发布管理
date: 2017-02-20 16:47:00 +0800
categories:
- Tech
tags:
- 运维
- SemVer

---

## 为什要进行软件版本控制

> 目的只有一个——更好地进行软件开发。

1. 识别每个软件版本。给发布的软件版本一个名称，可以识别不同的软件发布版本，便于追溯。
2. 记录软件的里程碑。项目中 Git 的提交记录非常多，使用软件版本就可以对特定的提交(commit)进行标记。
3. 对软件发布进行必要的说明。软件版本摘要看出本版本的更新内容。

比如我们要回滚软件到某一个状态，就可以对直接通过软件版本及其说明来确定要回滚的版本是哪一个。

----

## 在 Git 中使用基于 SemVer 的发布管理

### 发布版本控制的作用

对发布的版本进行必要的说明，如这次版本的简要说明、增加、更新的具体内容、影响范围等，来记录软件版本的演变。

简单讲就是对每次要发布的版本写上备注说明，说明这次是要发布原因、发布内容、影响范围等。这样就可以方便地了解到这发布的起因、经过和结果，同时也方便对发布版本进行追溯。

相关阅读：[为什么要使用语义化的版本控制？](http://semver.org/lang/zh-CN/#section-3)、[语义化版本控制 FAQ](http://semver.org/lang/zh-CN/#faq)。

### 软件版本号命名规则

市面上存在很多不同的软件版本号命名规则，Windows、Mac 和微信的软件版本命名规则就不一样，他们的目的都是一样的——管理软件版本。

Node.js 使用 [Semantic Versioning 2.0.0(语义化版本)](http://semver.org/)，简称为 SemVer，进行包版本的控制。

SemVer 风格的版本号命名规则：`MAJOR.MINOR.PATCH`

> 也可以使用自己制定的命名规则。

相关文章：

- [Semver: A Primer](https://nodesource.com/blog/semver-a-primer/)(must read!)
- [软件版本号——维基百科](https://zh.wikipedia.org/wiki/%E8%BB%9F%E4%BB%B6%E7%89%88%E6%9C%AC%E8%99%9F)


### Git 中使用 tag 进行版本标记

通过 Git 的 tag 功能，对生产分支中要发布的提交进行标记。

Git 增加 tag：

``` bash
$ git tag v1.0.0 
$ git tag v1.0.0 -m 'my version description message'
```

Git 删除 tag：

``` bash
$ git tag -d v1.0.0
$ git push origin :refs/tags/v1.0.0
```

查看 tag：

``` bash
$ git show v1.0.0
```

将 tag 推到远程仓库：

``` bash
$ git push origin v1.5 # 推送单个 tag
$ git push origin --tags # 推送所有的 tags
```

相关资料：

- [2.6 Git 基础 - 打标签](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

----

Robin on February 20, 2017 Moday at Office

> fine afternoon! with moderate sun light!