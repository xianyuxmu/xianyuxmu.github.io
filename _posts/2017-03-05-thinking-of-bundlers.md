---
layout: post
title: 前端打包工具思考
date: 2017-03-05 13:42:00 +0800
categories:
- Tech
tags:
- 前端

---


### 打包工具的是什么？作用是什么？

对我们而言，打包工具是一个前端开发的辅助工具，在前端开发的各环节中为我们提供了调试、mock、构建、预处理、混淆、压缩等一些列相关功能。

通常，打包工具从一个打包入口（或多个）开始，递归式地进行模块导入并对不同类型的码进行相应的处理，直到所有依赖的文件被放在一个（或多个）文件中。

同时，我们要清楚打包工具的出现及其提供的相应功能是有前置条件的，这些前置条件存在与否、作用的权重大小都会对之产生影响。这些前置条件对我的影响程度、这些前置条件现在的状态和未来的终态是什么样的，是我们一定要去关注和考量的。

### 不同打包工具的异同是什么？

对于前端而言，不同打包工具完成的功能是一致的，关键差异在于如何实现相关过程。一些关键的过程包括代码的编译方式（动、静态编译等）、压缩方式（gzip、混淆等）等，这些会受到语言发展、浏览器发展、JavaScript 引擎的影响。

从历史发展角度看伴不同时期出现了不同的打包工具，他们都是为解决我们前端工程化问题——模块化、可复用等。伴随着 JavaScript 本身及其相关领域的发展，我们还将看到新的打包工具不断出现。我们应该要明确的一点是，不同的打包工具完成的功能是一致的，知其不变，以应万变。

### 我们现在使用打包工具干什么？

简单讲，就是打包。展开点讲，有 Babel 转译、代码预处理、压缩等。

### 我们的关注点是什么？

从始至终不变的是，我们关注点一直在业务。

除了业务本身，我们还专注打包之后的源码大小、渲染速度、使用体验等。实际上，打包工具这个作用因子对整个业务过程（产品、研发、运营）来说，起作用的范围有限的——主要在开发、大包过程。多因素的作用之下，我们需要在更宏观的角度进行设计（架构分层），在微观处优化（算法、业务流程、兼容）。

### 未来的业务场景是什么？

打包工具有赖开发场景进行选择，开发场景是基于业务场景，所以了解未来的业务场景是关键。

> Robin on Mar. 05, 2017 13:42 at Wangjing CBD.