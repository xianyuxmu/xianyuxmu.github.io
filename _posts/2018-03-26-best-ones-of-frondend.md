---
layout: post
title: 前端精选
date: 2018-03-26 09:09:00 +0800
categories:
- Tech
tags:
- 前端

---

## 认识前端

- [前端工程——基础篇](https://github.com/fouber/blog/issues/10)

## 前端缓存

### 前端缓存分类

- 纯静态页面：直接放CDN。
- 纯动态页面：使用Redis或数据库缓存。
- 服务端模板引擎渲染的静态页面：
	- 数据做Redis缓存，缓存设置超时失效。
	- 渲染结果页面做redis缓存，缓存设置超时失效。

### 缓存相关的思考

- 缓存更新策略
	- 超时自动失效
	- 手动强制刷新
- 根据不同缓存场景，规划不同域名
	- 对数据实时性要求高的，不设置缓存。
	- 不变的，静态化。
	- 短时间不变的，短时间缓存。
- 做好缓存降级预案，出问题时可以立即关闭。

### 前端静态资源部署

- 域名规划: `https://static.yourdomain.com/:业务名/:子目录(可选)/<文件名>.<文件hash指纹>.js`
- 灾备预案：
	- CDN降级
	- 源站降级
	- 缓存刷新

参考：

- [大公司里怎样开发和部署前端代码](https://github.com/fouber/blog/issues/6)
	- [The Asset Pipeline](http://guides.rubyonrails.org/asset_pipeline.html)
	- [Asset Pipeline - 中文](https://ruby-china.github.io/rails-guides/v4.1/asset_pipeline.html)
- [大公司是怎么发布静态资源的](https://segmentfault.com/a/1190000007122250) 


----

## 精选文章

- [前端工程与性能优化](http://fex.baidu.com/blog/2014/03/fis-optimize/)
	- 优化方向：
		- 请求数量：合并脚本和样式表，CSS Sprites，拆分初始化负载，划分主域
		- 请求带宽：开启 GZip，精简 JavaScript，移除重复脚本，图像优化
		- 缓存利用：使用 CDN，使用外部 JavaScript 和 CSS，添加 Expires 头，减少 DNS 查找，配置 ETag，使 AjaX 可缓存
		- 页面结构：将样式表放在顶部，将脚本放在底部，尽早刷新文档的输出
		- 代码校验：避免 CSS 表达式，避免重定向
- [从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)
