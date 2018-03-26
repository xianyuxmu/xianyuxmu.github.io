---
layout: post
title: 前端精选
date: 2018-03-26 09:09:00 +0800
categories:
- Tech
tags:
- 前端

---

## 文章分类

### 性能优化

- [前端工程与性能优化](http://fex.baidu.com/blog/2014/03/fis-optimize/)
	- 优化方向：
		- 请求数量：合并脚本和样式表，CSS Sprites，拆分初始化负载，划分主域
		- 请求带宽：开启 GZip，精简 JavaScript，移除重复脚本，图像优化
		- 缓存利用：使用 CDN，使用外部 JavaScript 和 CSS，添加 Expires 头，减少 DNS 查找，配置 ETag，使 AjaX 可缓存
		- 页面结构：将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出
		- 代码校验：避免 CSS 表达式，避免重定向

### 前端大图

- [从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)