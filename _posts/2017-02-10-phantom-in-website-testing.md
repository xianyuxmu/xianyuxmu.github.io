---
layout: post
title: 基于 PhantomJS 的网站测试
date: 2017-02-10 16:58:00 +0800
categories:
- Tech
tags:
- PhantomJS
- Website Testing

---


## PhantomJS

### PhantomJS 是什么

> PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.


所以，现在还要看一下 [WebKit](https://webkit.org/) 是什么？WebKit 是一种用来让网页浏览器绘制网页的排版引擎。详细介绍请看 [Wikipedia](https://en.wikipedia.org/wiki/WebKit)。

简单讲，PhantomJS 是一个无界面、可执行脚本浏览器。从此出发，可以将 PhantomJS 用于很多的场景。

### PhantomJS 能做什么

1. 无界面网站测试。PhantomJS 不是一个测试框架，PhantomJS 只是通过合适的执行脚本启动测试过程。
2. 屏幕截取。WebKit 是一个网页布局、渲染引擎，所以通过 PhantomJS 可以打开一个网页或者根据 HTML 渲染出内容用于截屏。
3. 页面自动化。同市面上的多数浏览器一样，PhantomJS 支持加载、操作网页，所以 PhantomJS 支持 DOM 脚本、CSS 选择器。
4. 网络监控。PhantomJS 支持检查网络情况，所以可以用来分析网络行为和网络性能。

----


### PhantomJS 在前端测试的应用

下面看看 PhantomJS 是如何应用于前端开发中的[软件测试](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E6%B5%8B%E8%AF%95)。[前端自动化测试探索](http://fex.baidu.com/blog/2015/07/front-end-test/)，这篇文章介绍很详细，就不加赘述。

万变不离其宗，这些测试都是基于 PhantomJS 本身的能力以及其提供的能力来开展的。而 PhantomJS 能够发挥么样的作用，还要看具体的业务场景。

----

End by Robin February 10, 2017 Friday
