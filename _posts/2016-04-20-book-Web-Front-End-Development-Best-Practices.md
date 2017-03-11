---
layout: post
title: 《Web 前端开发最佳实践》 Notes
date: 2016-04-20 13:06:46 +0800
categories:
- Reading
- Tech
tags: 
- Web前端
- 技术
---

![book-Web-Front-End-Development-Best-Practices](/uploads/books/book-Web-Front-End-Development-Best-Practices.jpg)


书名[《Web 前端开发最佳实践》](https://book.douban.com/subject/26305106/)，作者：[党建](http://www.cnblogs.com/dangjian/p/4228313.html)


<!-- more -->


### 第一章 Web 前端开发概述

> 前端技术发展迅速，但是起步较晚。

沟通能力：前端工程师需要和 UI设计师 以及 后端工程师进行沟通。


### 第二章 高效的 Web 前端开发

代码合并、压缩。Gzip

HTML5的平稳降级：

[Modernizr](modernizr.com)-Respond to your user’s browser features.
[Can I use](http://caniuse.com/)
[html5shiv](https://github.com/aFarkas/html5shiv)-This script is the defacto way to enable use of HTML5 sectioning elements in legacy Internet Explorer. 

Web 性能： PageSpeed。

CSS 代码压缩。

图片相关：

[TinyPNG](https://tinypng.com/)
[JPEGMini](http://www.jpegmini.com/)

Mac:[ImageOptim](https://imageoptim.com/mac)


### 第三章 标准的 HTML 代码

[W3Validator](https://validator.w3.org/)

<meta>标签的妙用。


### 第四章 高可读性的 HTML

**样式和结构分离：**开发标签的时候要注意**语义**。不要使用文本标签来进行优化显示，删除干扰语义的标签。

- label 的 for 属性。
- h1、h2等的标签顺序。
- 有必要的话，加入 tabindex 顺序。
- 块状元素 和 行内元素


### 第五章 积极拥抱 HTML5

> Nothing!


### 第六章 高维护性的 CSS

- 采用 [Less](http://lesscss.org/) 和 [Sass](http://sass-lang.com/) 构建高可复用的的 CSS 代码。
- 合理使用 CSS 权重。尽量不要使用 ID 选择器。


### 第七章 高性能的 CSS

- 不要设置图片的尺寸。
- 使用 [CSS Sprite](http://www.w3schools.com/css/css_image_sprites.asp)




JavaScript：

- 配置数据和代码逻辑分离
- 采用 JavaScript 模版： [Mustache](https://mustache.github.io/)\[Underscore](http://underscorejs.org/)


DOM:

- DOM 的优化的总体原则是尽量减少 DOM 的操作。
- 前端性能优化的一个**主要**关注点是 ***DOM 操作优化***



个人总结：
这本书讲得很泛，具体内容没啥很有启发的思路。没啥实际nei



















