---
layout: post
title: pm2
date: 2017-03-15 20:11:00 +0800
categories:
- Tech
tags:
- pm2

---

## pm2 deploy


守护进程：[daemontools](https://cr.yp.to/daemontools.html)

定时备份：使用 [crontab](http://www.computerhope.com/unix/ucrontab.htm) 实现定时备份。crontab usages: [How to install crontab on Centos](http://stackoverflow.com/questions/21802223/how-to-install-crontab-on-centos)




### 多目标主机部署策略

发布过程分三步：发布机向目标部署机器发送指令 -> 目标部署机器开始初始化、构建、部署，完成后向发布机返回的发布状态 -> 发布机收到来自目标部署机器返回的结果，完成发布。

- 在发布机与多台目标部署机通过 ssh key 来实现 ssh 连接免登录，从而有能力操作目标部署机。
- 发布机的发布指令通过 ssh 传给目标部署机并在目标部署机子上执行。
- 目标部署机进行部署环境监测、通过 ssh key 去 Git 仓库拉去对应的分支、构建、部署，所有环节完成后，向发布机返回发布结果。

----

### pm2 Deployment References

- [Deployment - pm2](http://pm2.keymetrics.io/docs/usage/deployment/) - Official & professional guide for pm2's deployment(including the frequently troubleshooting). 
- [POD - git push deploy for Node.js](https://github.com/yyx990803/pod) - A Node.js deploy tool based on pm2 by Evan You(Creator of Vuejs)
- [Automating deployments to integrators](https://developer.github.com/guides/automating-deployments-to-integrators/) - GitHub Auto-Deploy Service using *Git Hook*
- Node applications' managers: [strong-pm](https://github.com/strongloop/strong-pm?_ga=1.59327021.1869727309.1487927902), [forever](https://github.com/foreverjs/forever) & [pm2](https://github.com/Unitech/pm2).

----

## pm2 监控

- [watchmen](https://github.com/iloire/WatchMen)
- [pm25](https://github.com/PaulGuo/PM25)
- [open-falcon](open-falcon.org)





