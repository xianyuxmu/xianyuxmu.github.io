---
layout: post
title: JavaScript 跨域请求
date: 2017-02-16 22:00:00 +0800
categories:
- Tech
tags:
- JavaScript
- Cross-Domain

---

跨域请求存在问题是因为浏览器存在安全策略，第一个是同源策略(`SOP, Same-Origin Policy`)，第二个是浏览器中不同域的框架(iframe)之间不能通过 js 进行交互。

同源策略的同源定义：如果协议(protocol)，端口(port)和主机(host)对于两个页面是相同的，则两个页面具有相同的源。


## 几个常见的跨域请求方案

1. 跨域资源共享(CORS，Cross-Origin Resource Sharing)。 跨域资源共享需要服务器端对 `CORS` 的支持，可以通过设置 `Access-Control-Allow-Origin` 来进行支持；客户端进行请求的时候需要设置 Header `mode: 'no-cors'`。支持所有的请求类型。
2. 通过 [JSONP](https://en.wikipedia.org/wiki/JSONP)(JSON with Padding) 进行跨域。JSONP 允许 `callback({"name","trigkit4"});` 这样的调用，`<script src="http://example.com/data.php?callback=dosomething"></script>` 在请求时服务器返回 `callback({})` 进行调用。JSONP 支持 GET 请求，不受同源策略的限制(`XMLHttpRequest` 受限制)，可以兼容旧的浏览器。
3. 通过修改 document.domain 来跨子域。把父页面的 `document.domain` 设置成同页面中的 iframe 的 `document.domain` 一样(主域必须一样)，这样就可以获取子窗口的 `window` 对象。
4. 使用 window.name 来进行跨域。一个 `window` 的生命周期内，窗口载入的所有页面共享一个 `window.name`。
5. 使用 HTML5 的 window.postMessage() 方法跨域。`window.postMessage(message,targetOrigin)` 方法是 HTML5 新引进的特性，可以使用它来向其它的 `window` 对象发送消息，无论这个 `window` 对象是属于同源或不同源，目前 IE8+、FireFox、Chrome、Opera 等浏览器都已经支持`window.postMessage()` 方法。

----

## 跨域请求存在的问题

1. 可能需要支持多套不同的鉴权策略。
2. 接入多个服务让项目更加复杂。数据约定等。


----

## 参考：

- [Cross-Domain requests in Javascript](https://jvaneyck.wordpress.com/2014/01/07/cross-domain-requests-in-javascript/)
- [详解 js 跨域问题](https://segmentfault.com/a/1190000000718840)

----

Robin on February 16, 2017