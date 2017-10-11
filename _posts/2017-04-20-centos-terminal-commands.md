---
layout: post
title: CentOS 终端命令
date: 2017-04-20 20:20:00 +0800
categories:
- Tech
tags:
- CentOS

---

## 常用命令参考

- [CentOS常用的文件操作命令总结](http://www.haorooms.com/post/centeros_wj_zj)
- [【CentOS命令】CentOS常用命令整理](http://blog.sina.com.cn/s/blog_e084ba2b0102wpl1.html)
- [Linux（Centos）常用命令，新手玩VPS必备技能](http://www.vps520.com/1.html)
- [命令 101](https://www.git-tower.com/learn/git/ebook/cn/command-line/appendix/command-line-101#start)
- [Shell Command Language](http://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html)


----

## 使用笔记

### 查看CentOS系统版本

rpm --query centos-release

cat /etc/centos-release

cat /etc/redhat-release

cat /etc/os-release

### 常用的命令

开启简易的端口，通过 ip 访问查看该目录下的文件：`python -m SimpleHTTPServer 8410`

查看 IP: `ifconfig`

查看执行路径： `which someCommand`，查看所有的执行路径：`which -a ruby`

path list: `/etc/paths`

在 `~/.zshrc` 中增加 `export` 记录，类似 `export PATH=${PATH}:~/Library/Android/sdk/tools:~/Library/Android/sdk/platform-tools` 然后执行 `source ~/.zshrc` 即可。

查看当前用户：`whoami`

导出执行路径：`PATH=/home/optiman2/phantomjs/bin:$PATH`

计算 MD5 哈希值：`md5sum your-file.tar.gz`

列出指定的端口的使用情况： `lsof -n -i4TCP:8081`

强制杀死进程：`kill -9 SOME_PID`

----

安装字体(CentOS install fonts):

查看字体： `fc-list`

查看字体文件：`fc-list : file`

字体安装： `fc-cache -f /usr/share/fonts/`，会把 (either /usr/share/fonts/local/ or /home/<user>/.fonts/) 下的字体全部加载到 /usr/share/fonts/

----

文件下载：

wget /tmp/myfile 'http://www.google.com/logo.jpg'

curl -o /tmp/myfile 'http://www.google.com/logo.jpg'

----

解压缩：

unzip package.zip -d /opt

To compress a file with zip, type the following:

`zip -r filename.zip files`


查找：

``` bash
$ find ./test -name "*.php"
./test/subdir/how.php
./test/cool.php

link: http://www.binarytides.com/linux-find-command-examples/

```

----

排序、去重：

`cat <your file> | sort | uniq`

----

### 串行执行

A pipeline is a sequence of one or more commands separated by the control operator `'|'`.

`[!] command1 [ | command2 ...]`, command1 命令的结果会被传到 command2 中。

### 文件链接命令 ln

ln命令是Linux中一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个同步的链接。

Linux文件系统中，有所谓的链接(link)，我们可以将其视为档案的别名，而链接又可分为两种 : 硬链接(hard link)与软链接(symbolic link)，硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。

- 硬连接: `ln TARGET LINK_NAME`
- 软链接: `ln -s TARGET LINK_NAME`



----

Robin on April 20, 2017 Thursday