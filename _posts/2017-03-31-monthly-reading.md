---
layout: post
title: March 2017 月读
date: 2017-03-31 22:00:00 +0800
categories:
- Reading
- Life
tags:
- 月读

---

## Life

- [This two-year-old is better at golf than you'll ever be](https://twitter.com/i/moments/847601041726099457)——我的小孩以后也要教他打高尔夫球 🏌️。

## Management

- [与新员工的“蜜月期”要这么过](https://zhuanlan.zhihu.com/p/22154125?refer=wowoohr)——持续的吸引力是关键。
- [李大学：CTO，应该像CEO一样思考](http://www.infoq.com/cn/articles/a-cto-should-think-like-ceo)
- [构建高效能团队](http://www.infoq.com/cn/presentations/build-high-performance-teams-azhu)
- [如何打造公司级公共前端团队——滴滴](https://github.com/DDFE/DDFE-blog/issues/2)
- [技术管理](https://blog.youxu.info/2015/05/17/tech-lead-1/)——提高工作效率、如何快速学习、如何进行技术管理。作者：*徐宥，之前在 Fitbit (领导机器学习团队) 和 Google (广告和无人驾驶) 干活。*
- [丰田的看板管理](https://en.wikipedia.org/wiki/Kanban)

## Business

- [都说在做某界的 Airbnb，Airbnb 到底做了什么](http://daily.zhihu.com/story/4826947)——介绍了 Airbnb，多图。
- [[创业] 解读Andreessen Horowitz风险基金和它宣称看好的16个创业方向](https://zhuanlan.zhihu.com/p/19944339?columnSlug=qinchao)
- [SM](http://www.smprime.com/)，创办者 [施至成](http://baike.baidu.com/item/%E6%96%BD%E8%87%B3%E6%88%90)，厦门 SM 城市广场的拥有者。
- [1 篇文章，1000 个潮包，一夜变现 40 万的内容实验](http://mp.weixin.qq.com/s?__biz=MjgzMTAwODI0MA==&mid=2651844386&idx=2&sn=de5c35c52b85c5c78fd3e5cc7c7ab44b)
- [滴滴 webapp 5.0 Vue 2.0 重构经验分享](https://github.com/DDFE/DDFE-blog/issues/13)——分模块开发、动态模块


----

## Tech

- [链家网高可用架构演进](http://www.infoq.com/cn/presentations/evolution-of-lianjia-high-available-architect)
- [隋剑锋：中国古典哲学在云计算架构设计中的应用](http://www.infoq.com/cn/interviews/interview-with-suijianfeng-talk-cloud-computing?utm_campaign=rightbar_v2&utm_source=infoq&utm_medium=interviews_link&utm_content=link_text) - 上善若水，系统应该可以像水一样拆离合融合。战略要站得高。云化。未来和现在的差异会很大，我们的演变要平滑。**云化过程中，业务和平台要能分开来。**企业要专注于业务。不同时期下，业务需求不一样，云的出现是因为现在业务的规模上去了。企业跟进主流是因为时代的变化太快，企业无法把握好未来的形态，只能跟进。需求不断驱动变化。未来云可以互联(亚马逊连接阿里云)。以后一套系统来解决全部的问题，不需要同时维护多个系统。未来的服务采用 SOA、路由，不需要去关注资源在哪。未来的路由是分布式的路由，不是集中性的，当然也有层级存在。未来的路由是多合一的，来减少层次，服务要跟着数据走。架构应该解决未来的问题。
- [谷歌宣布，PWA将获得与安卓原生应用同等的待遇与权限](http://www.infoq.com/cn/news/2017/02/PWA-Chrome?utm_source=infoq&utm_medium=popular_widget&utm_campaign=popular_content_list&utm_content=interview) - 更加轻量的开发。
- [度量驱动开发](http://www.infoq.com/cn/articles/metrics-driven-development)
- [Vue 2.0——渐进式前端解决方案](http://www.infoq.com/cn/articles/vue-2-progressive-front-end-solution?utm_campaign=rightbar_v2&utm_source=infoq&utm_medium=articles_link&utm_content=link_text) —— 介绍 Vue 最好的文章。
- [每个架构师都应该研究下康威定律](http://www.infoq.com/cn/articles/every-architect-should-study-conway-law) - organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations. -  ***[M. Conway](https://en.wikipedia.org/wiki/Conway%27s_law)***
- [从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)
- [协程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868328689835ecd883d910145dfa8227b539725e5ed000)—— Lua 线程。线程是通过调用内嵌函数 `coroutine.create(f)` 创建的一个协同例程 (co-routine)，其中 f 是一个 Lua 函数。线程不会在创建时启动；相反，它是在创建之后使用 `coroutine.resume(t)` 启动的，其中 t 就是一个线程。每个协同例程都必须使用 `coroutine.yield()` 偶尔获得其他协同例程的处理器。
- [React+Redux Demo 和总结](https://github.com/bailicangdu/react-pxq)——讲得挺好的。
- [饿了么类似的 SPA —— 基于 Vue](https://github.com/bailicangdu/vue2-elm)
- [令人激动的微服务2.0技术栈](https://mp.weixin.qq.com/s?__biz=MzA5Nzc4OTA1Mw==&mid=2659599064&idx=1&sn=07655af084819377d69fc937dc558a0c)——关键词：部署，交付，API，版本控制，合同，缩放/自动缩放，服务发现，负载均衡，路由/自适应路由，健康检查，配置，熔断器，bulk-heads，TTL / deadlining，延迟跟踪，服务因果跟踪，分布式日志，度量操作与收集。
- [App Shell 模型](https://developers.google.com/web/fundamentals/architecture/app-shell)
- [30 VR Projects In 30 Days](https://risonsimon.com/days-in-vr/)—— VR 项目。
- [Introducing React Loadable](https://medium.com/@thejameskyle/react-loadable-2674c59de178#.unr5hyb1o)——延迟加载。
- [前端开发的历史和趋势](https://github.com/ruanyf/jstraining/blob/master/docs/history.md) —— 阮一峰的文章。简要介绍前端的发展。
- [技术高手如何炼成](https://zhuanlan.zhihu.com/p/20270317?columnSlug=zhengyun)——[网友评论](https://www.zhihu.com/question/37757196/answer/73753008?from=timeline&isappinstalled=0)：**“天天鼓吹让人做技术达人能显得你高大上是吗？你的人生，除了什么狗屁的知识体系，还值得更多。千万不要和这些当了ceo没事写个代码的人天天吹你应该怎么给他们卖命，那是嫌日子太好过了，他们的屁股和我们是对立的！你赔掉的人生，都给他们赚走了。”**

----

## Front End

- [JavaScript的抽象语法树与语法解析](http://wwsun.github.io/posts/javascript-ast-tutorial.html)——简称“AST”，Abstract Syntax Tree。
- [一道滴滴的负载均衡前端面试题](https://zhuanlan.zhihu.com/p/25600864?group_id=822604136280395776)——通过一致哈希来形成一个虚拟机器集群，这个集群的 hash 值之间的映射关系是固定的，可以解决端到端的通讯。

## Thinking

- Your world is as big as you make it.
- [Give it five minutes](https://signalvnoise.com/posts/3124-give-it-five-minutes) —— 对话、交谈时反对一个人的观点很正常，而即使你的观点是正确的，但在反驳之前，请让对方把话讲完。
