---
layout: post
title: JavaScript 简史
date: 2017-02-17 10:38:00 +0800
categories:
- Tech
tags:
- JavaScript

---

## JavaScript 简史

1995年05月，[Brendan Eich](http://en.wikipedia.org/wiki/Brendan_Eich) 在10天内设计出 JavaScript。原先还被叫做 Mocha、LiveScript。

1995年09月，因为商标问题，JavaScript 的叫法被放弃。但我们还是称之为 JavaScript。

1996年-1997年，JavaScript 交给 [ECMA - European Computer Manufacturer's Association](http://en.wikipedia.org/wiki/Ecma_International) 来执行一套标准，这个阶段出了第一份标准 ECMA-262 Ed.1(历史上最著名的 JavaScript 实现版本)，此时 ECMAScript 成为官方叫法。

标准制定不断进行，1998年发布 ECMAScript 2、1999年出了 ECMAScript 3，并成为现代 JavaScript 的基线。

2000年，ES4 开始提上 ECMA 的标准议程。微软开始有意参加，最终无意参加。

2003年，ES4 标准制定被推迟。

2005年是关键的一年，two big events。 Brendan Eich 和 Mozilla 重新加入 ECMA，重启了 ECMAScript 4 的制定议程，并以 ActionScript 3 的标准为 ES4 标准设计目标。同年，ES4 的制定过程充满坎坷。

2005年，Ajax 的发展极大推动了 JavaScript 的发展，引起了网页动态应用的兴起，也就是早期的 SPA。开源库兴起，出现了 jQuery、Dojo、Mootools，JavaScript 随之复兴。

2008年07月，在 Oslo 的一次会议推进了 JavaScript 标准制定的进程。

2009年， ECMAScript 3.1 被重命名为 ECMAScript 5，[Harmony](http://ejohn.org/blog/ecmascript-harmony/) 议程推进了 JavaScript 的发展。Hormony 是引领者，不是跟随者。

2015年，ECMAScript 6 发布。

至今，ES7(ECMAScript 2016) 正在制定中。

----

### 要点

1. ECMAScript(also ActionScript) 是标准化的 JavaScript。
2. 各种浏览器内核支持了 ECMAScript 的接口，但具体的实现不尽相同。


### References

- [ES5, ES6, ES2016, ES.Next: What's going on with JavaScript versioning?](https://benmccormick.org/2015/09/14/es5-es6-es2016-es-next-whats-going-on-with-javascript-versioning/)
- [A Short History of JavaScript](https://www.w3.org/community/webed/wiki/A_Short_History_of_JavaScript)


## Browser 简史

### Early 1990s

The first web browser, WorldWideWeb, was developed in 1990 by Tim Berners-Lee for the NeXT Computer.

### Late 1990s

Microsoft 大战 Netscape，Netscape 战败市场份额缩小后，开源 Mozilla。

### 2000s

2002年 Firefox 推出，2003年微软宣布不再开发 standalone 版本的 IE。

2008年，Google 推出基于 V8 引擎的 Chrome 浏览器，并快速迭代更新(evergreen application)。V8 也推动了 Node.js 的发展，成为了 Node.js 的核心。

### 要点

1. IE9 是现代浏览器的分水岭。BBC 网站管理员说过，现代浏览器至少要支持 `document.querySelector()/document.querySelectorAll()`、`window.addEventListener()` 和 `Storage API - localStorage & sessionStorage`。
2. 相对于 FireFox、Chrome，IE 的更新是个缓慢的过程。

more info, visiting [History of the web browser](https://en.wikipedia.org/wiki/History_of_the_web_browser).

----

## 前端划时代的几点

### 库的崛起

1. JavaScript库是一种扩展，为现有的程序提供附加的功能。
2. Library 形成生态，帮助开发者磨平浏览器实现、功能差异。jQuery 是最流行的库之一。

### 移动端的兴起

智能手机兴起，移动设备体量不断膨胀，移动 web 浏览的比例正在不断上升。

----

> contents above are written on February 20, 2017 
