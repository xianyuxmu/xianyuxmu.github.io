---
layout: post
title: Mac 中文字符显示为乱码
date: 2016-04-23 14:25:58 +0800
categories:
- Tech
tags:
- Bug
- Mac字符乱码
---

mac displays chaotic code with some character
### 问题描述：

大约前两周(今天是2016年04月23日)开始，使用 Mac 时，输入中文冒号“：”时会显示成一个类似[阿拉伯文字](https://zh.wikipedia.org/wiki/%E6%96%87%E5%AD%97#/media/File:WritingSystemsoftheWorld.png)的字符。类似下面这样，总之是显示成“乱码”了。

![中文冒号显示成这样子](/uploads/solution/solution-mac-displays-chaotic-code-with-chinese-character/3.png)

<!-- more -->

出现乱码的情况：
使用 Mac 自带邮件应用发邮件的时候，敲打邮件正文的时候“：”字符乱码。
使用 Chrome 时，在地址栏输入字符“：”显示正常，网页内容显示“：”字符为乱码。

我打电话给 Apple 的技术支持，Apple 的技术专家远程帮助我进行修复。安全模式启动 Mac，重置 Mac 设置，这两个不管用。没办法，Apple 专家建议建立新的管理员帐号试试，发现中文冒号“：”乱码可以正常显示了。但是因为做开发，在新用户下有些开发环境要重新建立。因而我再次打电话给 Apple 技术专家询问能否把我的开发环境从就的管理用户转移过来，被告知“不行”。Apple 专家没辙了。

于是，我尝试自己看看能不能解决。

我回忆了大概是两周前出现了这种情况，那么我在那个时候是做了什么事呢？才想起我安装了个字体：[Lucida Grande Regular](http://www.911fonts.com/font-download/download_LucidaGrandeRegular_6216.htm)。当时在一篇文章中见到这个字体，觉得很漂亮，也是就下载安装了。我尝试删掉这个字体，重新打开 Mac 自带的 邮件 应用发现中文乱码“：”显示正常了。问题解决。

下面我将解释为什么会出现这个问题：

首先请下载下载字体：[Lucida Grande Regular](http://www.911fonts.com/font-download/download_LucidaGrandeRegular_6216.htm)，点击安装，出现以下：
![Mac font installation](/uploads/solution/solution-mac-displays-chaotic-code-with-chinese-character/1.png)

出现了***“重复字体”***！！！

安装完之后查看该字体，提示信息**“已安装了此字体的多个副本”**：
![Mac font installation](/uploads/solution/solution-mac-displays-chaotic-code-with-chinese-character/2.png)

原因就是这个。

最后，我继续使用这个管理员账号进行学习、开发等。接着，我把这个问题的解决方式告诉了 Apple 专家 好让以后大家出现类似问题的时候可以在 Apple 专家 那边得到解决。同时，我收到一封来自 Apple 的感谢信！哈哈～



相关文章：
[如何排查Mac OS X上的字体问题](https://blogs.adobe.com/CCJKType/2009/08/troubleshoot.html):

``` bash
确定字库是否安装到正确的目录
Mac OS X包含了五个具有不同用途的字库文件夹，而且OS X允许同一个字体重复安装到多个字体文件夹中。如果有重复的字体存在，Mac OS X不会区分字体格式上的不同，会将其看作同一个字体，其读取字库数据的优先顺序如下:
– Users/[user name]/Library/Fonts
– Library/Fonts
– Network/Library/Fonts
– System/Library/Fonts (不要修改该目录下的字库，该目录下存放了Mac OS X系统字库，更多的内容请参考)
– System Folder/Fonts（该目录用于支持 Classic, Carbon, 和Cocoa的程序）
```
[MAC OS X 下 R 语言作图中文字符显示乱码问题](http://not.farbox.com/post/chinese-character-font-mac-rlan)

[乱码－Wikipedia](https://zh.wikipedia.org/wiki/%E4%BA%82%E7%A2%BC):

``` bash
可能的产生原因:
1. 一般是软件程序解码错误。
2. 字体文件（font file）不对。
3. 来源编码错误，或文件受到破坏。
4. 一种语言版本的操作系统安装了另外一种语言版本的应用程序，或者应用程序安装的补丁的语言版本与应用程序原来安装的语言版本不一致。
5. 早期单字节的应用程序在打开双字节语言的文件时不能正确识别文字的分区，在换行的地方把一个字从中分成两段，导致紧接在后面的整个一行全部都是乱码。
6. 低版本的应用程序不能识别高版本的程序创建的文件。

```


-EOF-





