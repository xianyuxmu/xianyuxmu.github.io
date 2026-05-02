---
layout: post
title: JavaScript Events
date: 2017-02-17 17:03:00 +0800
categories:
- Tech
tags:
- JavaScript

---

JavaScript 中事件的概念经过多年的发展，目前已经达到可靠、可用的稳定阶段了，而事件处理 API 也实现了跨浏览器标准化(大部分是 API)。

JavaScript 通过事件将一切黏合在一起。从 DOM 中产生的事件由 JavaScript 代码来处理，JS 和 DOM 的综合运用是所有现代 Web 应用的基础。

## 栈、队列和事件队列

JavaScript 所运行的代码中，无论是全局上下文中的函数，还是被函数调用的函数，都可以描述成**栈**。

只要栈为空，就会去**队列**中挑选新的代码来执行，**栈和队列**对于掌握时间来说至关重要。

除此之外，还有第三个要素：**堆**。堆是变量、函数一起其他命名对象所存在的区域，当 JS 需要访问对象、函数或变量是，它就会去堆中获取要访问的数据。

> 请记住，除了 Web Worker 之外，JavaScript 都是单线程的。

了解 JavaScript 的事件循环，可以查看这篇文章[《JavaScript 运行机制详解：再谈Event Loop》](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)，浅显易懂、更加直观的视频讲解 [Philip Roberts: Help, I’m stuck in an event-loop.](https://vimeo.com/96425312)，事件循环的 DEMO 请访问 [loupe](http://latentflip.com/loupe)。

注意点：

1. 即时在 JavaScript 线程阻塞的时候触发的 UI 事件没反应(点击按钮没效果)，该事件**也会被加入到事件队列**中。
2. JavaScript 事件队列中事件的执行是按顺序的。

### 事件循环

事件循环需要浏览器中两个线程之间的合作：**事件跟踪线程**和**JavaScript线程**。

两个线程共同捕获事件，并依据是否注册过事件处理程序来对其进行归类。这个过程叫做**事件循环**。

![how-to-traverse-dom](/uploads/tech/javascript/javascript-event-loop.png)

1. 每次循环开始，都会检查是否注册过事件处理程序，如果没有什么都不做；如果有，事件循环会将其放入 JavaScript ***队列的首部***，使得 **JavaScript 可以第一时间**

### 事件阶段

JavaScript 事件分为两个阶段执行：**捕获阶段**和**冒泡阶段**。在一次事件中，能够处理该事件的元素以及处理顺序都是不固定的。现代浏览器允许将事件处理程序放置于两个阶段。

## 事件侦听器

### 一、传统式绑定

1. 直接写在 HTML 元素的属性中，如 `<a onclick="openLink()"></a>`。
2. 还有就是 `document.getElementById('some-id').onClick = function(event){}` 绑定事件。

传统绑定方式兼容性较好，但只能使用事件冒泡，而不能切换成事件捕获。

### 二、DOM 绑定：W3C

这是 W3C 规定的事件绑定的**唯一标准的手段**。

1. 使用 `Element.addEventListener('click', function fn(e) {})` 绑定事件处理程序。
2. 使用`Element.removeEventListener('click', fn)` 一处事件绑定。


要点：

1. 这种绑定方式支持事件处理的**捕获阶段**，也支持事件处理的**冒泡阶段**。通过 `Element.addEventListener('click', function(){}, true)` 中第三个变量来确定，默认为 `false` 表示事件冒泡阶段，`true` 表示事件捕获阶段。
2. 事件处理函数中 `this` 指向当前元素，和传统事件处理一样。
3. `event` 总是做为第一个可用参数。
4. 可以绑定任意多的事件不会覆盖之前绑定的事件处理程序。
5. W3C 事件绑定唯一的缺点是：**不能工作在 IE8 及更低的版本中**。
6. 通过 `e.stopPropagation()` 取消事件冒泡，通过 `e.preventDefault()` 屏蔽默认行为。


### 事件委托

把重复的子元素的事件处理程序绑定到其父元素上，在冒泡阶段捕获。

----

## 事件对象

所有的事件处理函数内部都有 `event` 对象可以使用。通过事件对象可以知道：事件的种类、事件从哪里产生、单击处所在的坐标或者按下了哪些按键。

1. `event.type` 事件的种类: mouseover、click 等。
2. `event.target` 包含触发事件元素的引用。
3. `e.stopPropagation()` 阻止事件冒泡(或者捕获)。
4. `e.preventDefault()` 屏蔽默认行为。
5. 鼠标属性：pageX、pageY、clientX、clientY、layerX、offset、button(click、mousedown、mouseup)。
6. 键盘属性：crtlKey、keyCode、shiftKey。

----

## 事件类型

1. 加载及错误事件。load、beforeunload、error、resize、scroll、unload。
2. UI 事件。focus、blur。
3. 鼠标事件。click、dbclick、mousedown、mouseup、mouseover、mousemove、mouseout、mouseenter、mouseleave。
4. 键盘事件。keydown、keypress、keyup。
5. 表单事件。select、change、submit、reset。

----

> 读完 **Pro JavaScript Techniques, 2ed Edition**，结束了 JavaScript 深入阶段。

Robin on February 17, 2017 17:51 at Maoyan corridor.