---
layout: post
title: Docker Swarms
date: 2017-09-16 14:20:00 +0800
categories:
- Tech
tags:
- Docker
- Container
- Swarms

---

Demo: [devops](https://github.com/xianyuxmu/devops)


## Docker Swarms

- Tutorial: [Get Start - Docker](https://docs.docker.com/get-started/)


## Docker Links

- [Legacy container links](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)
	- [Docker container networking](https://docs.docker.com/engine/userguide/networking/)

`link` 的本质是通过修改 `/etc/hosts` 实现 `link`，参考如下代码：

``` bash
$ docker run -t -i --rm --link db:webdb training/webapp /bin/bash

root@aed84ee21bde:/opt/webapp# cat /etc/hosts

172.17.0.7  aed84ee21bde
. . .
172.17.0.5  webdb 6e5cdeb2d300 db

```

Codes above come from [Legacy container links](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)
