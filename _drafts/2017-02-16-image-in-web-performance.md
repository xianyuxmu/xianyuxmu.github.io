---
layout: post
title: 图片 - 网络性能优化
date: 2017-02-16 22:00:00 +0800
categories:
- Tech
tags:
- 网络性能优化

---


## 场景一: 图片按需加载

常见的类似微信文章的网页往往有很多的图片，一次性加在所有的图片会增加服务器的压力，同时也没必要。

### 进入视野的图片再加载

1. 默认图片通过 div 标签以及 CSS 的 `background-image` 实现。
2. 把 <img /> 标签的 `src` 属性去除，加上 `data-src` 属性，如：`<img data-src="picture-url"/>`，监听图片的滚动事件 `scroll`，当图片进入视野的时候，把 `<img />` 中 `data-src` 的值赋给 `src` 属性。

----
