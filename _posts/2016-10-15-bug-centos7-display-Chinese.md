---
layout: post
title:  "阿里云ECS CentOS 7 在 ssh 下中文乱码"
date:   2016-10-15 13:56:58 +0800
categories:
- Technology
tags:
- Bug
- 阿里云
---

今天升级镜像到 CentOS 7，接着使用 ssh 登录到主机发现中文显示乱码。

折腾了一小会，找出原因是本地终端 bash 与服务器终端 bash 的编码字符集不一样引起的。

<!-- more -->

解决方法如下：

## 服务器配置：

第一步，修改服务器本地化配置（`locale.conf`）：

路径：/etc/locale.conf

把：

```
LANG="en_US.UTF-8"
```
修改为：


```
LANG="en_US.UTF-8"
SUPPORTED="zh_CN.UTF-8:zh_CN:zh:en_US.UTF-8:en_US:en"
SYSFONT="lat0-sun16"
```

第二步，修改 `~/.bashrc`：

路径：~/.bashrc

增加以下代码：

```
export LANG='UTF-8' export LC_ALL='en_US.UTF-8'
```
然后运行命令：

```
	bash
```

下次登录时， .bashrc 文件自动运行，中文就会正常显示了。

## 本地配置：

本地配置和服务器配置一样，省去第一个步骤。

---

## 另外：

如果使用了 `oh-my-zsh`(我就是)，直接连接 `ssh` 的话也可能因为终端字符集不匹配而出现中文乱码，解决方式就是设置 `.zshrc` ：

编辑 `.zshrc`:

```
vim ~/.zshrc

#### 增加以下内容：

export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8  

```

接着，运行：

```
source ~/.zshrc
```

输入：

```
local

##### 可以看到：

LANG="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
```

再次登录 `ssh`，就可以正常显示了！

