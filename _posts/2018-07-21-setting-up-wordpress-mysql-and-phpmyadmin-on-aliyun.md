---
layout: post
title: 阿里云Swarm容器服务搭建WordPress、MySQL和phpmyadmin
date: 2018-07-21 16:06:00 +0800
categories:
- Tech
tags:
- 阿里云
- 2018年07月

---

使用“阿里云Swarm容器服务”，可以在10秒内搭建起WordPress系统。


## 搭建方案

### 安装步骤与编排模版

- 第一步：安装MySQL。
	- 启动`db-mysql`容器，设置主机名(hostname)为`db_mysql_server`，MySQL账户名默认为`root、`密码为`some-passwrod`。同时使用`volumes`，数据库数据存储路径`/var/lib/mysql`映射到机器路径`db_mysql_data`。
- 第二步：安装phpmyadmin。
	- 设置环境变量`PMA_HOST=db_mysql_server`，自定义路由前缀`pma`，通过`pma.<你的域名>.com`访问。
- 第三步：安装WordPress。

编排模版如下👇

``` bash
db-mysql:
  image: mysql:5.7
  restart: always
  hostname: db_mysql_server
  volumes:
     - db_mysql_data:/var/lib/mysql
  environment:
    MYSQL_ROOT_PASSWORD: some-passwrod
phpmyadmin:
  image: phpmyadmin/phpmyadmin
  restart: always
  ports:
    - 80
  environment:
    PMA_HOST: db_mysql_server
  labels:
    aliyun.scale: 1
    aliyun.routing.port_80: pma
    aliyun.depends: db-mysql
wordpress:
  image: wordpress:4.9.7-php7.2-apache
  restart: always
  ports:
    - 80
  environment:
    WORDPRESS_DB_HOST: db_mysql_server
    WORDPRESS_DB_PASSWORD: some-passwrod
  labels:
    aliyun.scale: 1
    aliyun.routing.port_80: wordpress
    aliyun.depends: db-mysql

```

### 容器集群容器网络互通

> 容器编排中最关键的就是集群内容器的网络互通，可以参考以下文章。

- [使用 Docker Compose 测试集群网络连通性](https://help.aliyun.com/document_detail/47621.html)
- [跨主机互联的容器网络](https://help.aliyun.com/document_detail/26030.html)
- [容器间的互相发现](https://help.aliyun.com/document_detail/26031.html)


## 本地Docker安装WordPress、MySQL和phpmyadmin

- 在本地Docker安装WordPress，直接参考：[Setting up WordPress with Docker](https://cntnr.io/setting-up-wordpress-with-docker-262571249d50) 👍


## 相关资料

- [Setting up WordPress with Docker](https://cntnr.io/setting-up-wordpress-with-docker-262571249d50) - 在本地Docker安装WordPress
- [Docker 微服务教程——以WordPress为例](http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
- [使用 OSSFS 数据卷实现 WordPress 附件共享](https://help.aliyun.com/document_detail/47637.html)
- 镜像相关文档
	- [phpmyadmin/phpmyadmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin/)
	- [mysql/mysql-server](https://hub.docker.com/r/mysql/mysql-server/)
	- [wordpress](https://docs.docker.com/samples/library/wordpress/)
