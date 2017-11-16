---
layout: post
title: Flexbox Layout
date: 2017-11-16 20:00:00 +0800
categories:
- Tech
tags:
- CSS

---

## 顶层思考

- 我们出发点是是为了实现布局的需求，我们可以采用不同的布局方式所提供了一定布局能力，来满足、实现我们的布局需求。
- 布局方式没有好坏之分、各有其优势和限制，但有灵活性高低、能力大小之分，最终一切要跟随实际需求来权衡。
- 各种布局可以混合使用。
- 而对于CSS而言，浏览器之间对不同布局方式实现程度很关键。

### 常见布局介绍

First we used tables, then floats, positioning and inline-block, but all of these methods were essentially hacks and left out a lot of important functionality (vertical centering, for instance)

- 网格布局(Grid Layout)
	- 关键特性：擅长二维布局、语义化较强的
	- 核心思想：二维布局、Grid布局模版同时特定元素可制定
	- 关键语法：`grid-column`、`grid-row`、`grid-template-columns`、`grid-template-rows`、`grid-template-areas`、`grid-area`
	- 适用范围：适用于复杂布局
	- Grid is for layout of items in two dimensions – rows AND columns.
- 弹性布局(Flexbox layout)
	- 关键特性：擅长一维布局(一行或一列)
	- 适用范围：适用于简单布局
	- Flexbox is essentially for laying out items in a single dimension – in a row OR a column.
- 浮动布局(float-based layout)
- 表格布局(Table-based Layout)

## 弹性布局(Flexbox Layout)

弹性布局是W3C在2009年提出了一种新的方案----Flex 布局，可以简便、完整、响应式地实现各种页面布局。

弹性布局是一种常见的CSS布局，在`flexbox布局`中一般包含`容器container`和`列表项item`。中文的叫法`父容器和子容器`，我觉得不如英文的`container & item`准确。

``` css
.container{
    display:-webkit-flex;
    display:-moz-flex;
    display:flex;
    display:-ms-flexbox;
}
```

### 关键词

- 父容器
- 子容器
- container
- item
- 剩余空间

### flex总结

1. 剩余空间＝父容器空间－子容器1.flex-basis/width - 子容器2.flex-basis/width - …
2. 如果父容器空间不够，就走压缩flex-shrink，否则走扩张flex-grow；
3. 如果你不希望某个容器在任何时候都不被压缩，那设置flex-shrink:0；
4. 如果子容器的的flex-basis设置为0(width也可以，不过flex-basis更符合语义)，那么计算剩余空间的时候将不会为子容器预留空间。
5. 如果子容器的的flex-basis设置为auto(width也可以，不过flex-basis更符合语义)，那么计算剩余空间的时候将会根据子容器内容的多少来预留空间。

from [深入理解css3中的flex-grow、flex-shrink、flex-basis](http://zhoon.github.io/css3/2014/08/23/flex.html)

### 属性介绍

- 容器(flex容器)
	- display: 
		- flex: 定义一个`flex容器`
		- inline-flex: 定义一个`内联flex容器`
		- CSS的`columns`属性对`display:flex;`无影响
	- flex-direction: 
		- row: `从左到右`行显示
		- row-reverse: `从右到左`行显示
		- column: `从上到下`列显示
		- column-reverse: `从下到上`列显示
	- flex-wrap: 
		- nowrap(默认)：不换行。*By default, flex items will all try to fit onto one line.*
		- wrap：换行，第一行在上方
		- wrap-reverse：反向换行，第一行在下方
	- flex-flow：
		- `flex-flow`是`flex-direction`和`flex-wrap`的简写，例如：`flex-flow: row wrap;`
	- justify-content：
		- flex-start(默认)：对齐到`flex-direction`定义的行开始方向
		- flex-end：对齐到`flex-direction`定义的行结束方向
		- center：居中
		- space-between：first item is on the start line, last item on the end line.
		- space-around：all the items have equal space on both sides.
		- space-evenly：items are distributed so that the spacing between any two items (and the space to the edges) is equal.
	- align-items：
		- baseline： 基于文本对齐。Items are aligned such as their baselines align.
		- flex-start：对齐到flex布局开始行
		- flex-end：对齐到flex布局结束
		- center：上下居中
		- stretch(默认)：拉伸到充满容器
	- align-content：
		- flex-start：对齐flex容器内所有行到felx容器开始行
		- flex-end：对齐flex容器内所有行到felx容器结束行
		- center：居中flex容器内的所有行
		- space-between：首行置顶，末行置尾；行之间均匀分布
		- space-around：所有行的上下两边拥有一样的边距
		- stretch(默认)：所有行拉伸到充满容器
		- ***Note***: this property has no effect when there is only one line of flex items.
- **列表项(flex容器子元素)**
	- order：
		- (默认情况)：`0` by default, flex items are laid out in the source order. 
		- 属性值需为整数，正负整数都是可以的
	- flex-grow：
		- 说明：This defines ***the ability for a flex item*** to grow if necessary.
		- 为一个数字，决定item的增大(grow)的能力大小，数值占行的比重越大，增大能力越强。如果存在剩余空间，按照`flex-grow`权重来分配。
	- flex-shrink：
		- 说明：This defines the ability for a flex item to shrink if necessary.
	- flex-basis：
		- 说明：This defines the **default size** of an element before the remaining space is distributed. 对于剩余空间的再分配，`flex-basis`比`flex-grow`优先
		- `flex-basis`: <length> | auto; /* default auto */
		- 这个属性值的作用也就是width的替代品，`flex-basis`的优先级比`width`高，会覆盖`width`
	- flex：
		- 说明：This is the shorthand for `flex-grow`, `flex-shrink` and `flex-basis` combined. Default is `0 1 auto`. **It is recommended that you use this shorthand property**
	- align-self:
		- 说明：This allows the default alignment (or the one specified by `align-items`) to be overridden for individual flex items.
		- align-self: auto | flex-start | flex-end | center | baseline | stretch;
		- ***Note that `float`, `clear` and `vertical-align` have no effect on a flex item.***


## 相关文章

- [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) - ***must read!*** diagrams & demos!
	- [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
- [深入理解css3中的flex-grow、flex-shrink、flex-basis](http://zhoon.github.io/css3/2014/08/23/flex.html)
- [Learn CSS Layout](http://learnlayout.com/)
- [学习CSS布局](http://zh.learnlayout.com/)
- [使用 CSS 弹性盒子](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)
- [An Introduction to the CSS Grid Layout Module](https://www.sitepoint.com/introduction-css-grid-layout-module/)
- [Does CSS Grid Replace Flexbox?](https://css-tricks.com/css-grid-replace-flexbox/) - Grid is much newer than Flexbox and has a bit less browser support.