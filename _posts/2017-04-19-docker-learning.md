---
layout: post
title: Docker 学习
date: 2017-04-19 20:10:00 +0800
categories:
- Tech
tags:
- Docker

---



## Docker 简介

- [What is Docker?](https://www.docker.com/what-docker)
- [Docker五大优势：持续集成、版本控制、可移植性、隔离性和安全性](http://dockone.io/article/389)／原文：[5 Key Benefits of Docker: CI, Version Control, Portability, Isolation and Security](https://dzone.com/articles/5-key-benefits-docker-ci)


## Docker 入门教程

- [Get started with Docker](https://docs.docker.com/get-started/)
- [基于 Docker 开发 NodeJS 应用](https://www.oschina.net/translate/develop-a-nodejs-app-with-docker)
- [Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
- [Docker Engine user guide](https://docs.docker.com/engine/userguide/)


![](https://i2.wp.com/blog.docker.com/wp-content/uploads/2015/11/image00.png?w=1887&ssl=1)

from [*DEPLOY AND MANAGE ANY CLUSTER MANAGER WITH DOCKER SWARM*](https://blog.docker.com/2015/11/deploy-manage-cluster-docker-swarm/)

![Docker's Container diagram](https://www.docker.com/sites/default/files/Container%402x.png)

----

## Docker 基本使用

- 查看 docker 进程：`docker ps`
- 进入一个 Container：`docker exec -it <Container Name> sh`
- 列出容器的端口映射：`docker port CONTAINER`，设置端口映射：`docker port CONTAINER [PRIVATE_PORT[/PROTO]]`
- 启动容器：`docker start my_container`
- 停止容器的运行：`docker stop my_container`
- Mac 中的 Images 存储路径，通过应用 [Docker for Mac](https://docs.docker.com/docker-for-mac/) 的 `Preferences` 来查看，类似：`/Users/<user name>/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux`。