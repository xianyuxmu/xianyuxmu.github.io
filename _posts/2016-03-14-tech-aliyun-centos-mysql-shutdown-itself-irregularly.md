---
layout: post
title: "阿里云服务器 MySQL 经常自动停止挂掉重启的完美解决方式"
date: 2016-03-14 12:07:53 +0800
categories:
- Technology
tags:
- Bug
- MySQL
- 阿里云ECS
---


关键词：

1. 阿里云服务器 MySQL 经常自动停止、挂掉、重启。
2. 阿里云CentOS WordPress 运行经常数据库连接不上。
3. CentOS + nginx + PHP + MySQL.

<!-- more -->

打开 MySQL 的 error.log 错误信息，在阿里云 CentOS 的路径为 /alidata/log/mysql/error.log,如下：

``` bash

2016-03-13 00:16:37 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2016-03-13 00:16:37 30576 [Note] Plugin 'FEDERATED' is disabled.
2016-03-13 00:16:37 30576 [Note] InnoDB: Using atomics to ref count buffer pool pages
2016-03-13 00:16:37 30576 [Note] InnoDB: The InnoDB memory heap is disabled
2016-03-13 00:16:37 30576 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2016-03-13 00:16:37 30576 [Note] InnoDB: Memory barrier is not used
2016-03-13 00:16:37 30576 [Note] InnoDB: Compressed tables use zlib 1.2.3
2016-03-13 00:16:37 30576 [Note] InnoDB: Using Linux native AIO
2016-03-13 00:16:37 30576 [Note] InnoDB: Using CPU crc32 instructions
2016-03-13 00:16:37 30576 [Note] InnoDB: Initializing buffer pool, size = 64.0M
2016-03-13 00:16:37 30576 [Note] InnoDB: Completed initialization of buffer pool
2016-03-13 00:16:37 30576 [Note] InnoDB: Highest supported file format is Barracuda.
2016-03-13 00:16:37 30576 [Note] InnoDB: The log sequence numbers 52296883 and 52296883 in ibdata files do not match the log sequence number 93636989 in the ib_logfiles!
2016-03-13 00:16:37 30576 [Note] InnoDB: Database was not shutdown normally!
2016-03-13 00:16:37 30576 [Note] InnoDB: Starting crash recovery.
2016-03-13 00:16:37 30576 [Note] InnoDB: Reading tablespace information from the .ibd files...
2016-03-13 00:16:37 30576 [Note] InnoDB: Restoring possible half-written data pages 
2016-03-13 00:16:37 30576 [Note] InnoDB: from the doublewrite buffer...
InnoDB: Last MySQL binlog file position 0 9828, file name mysql-bin.000335
2016-03-13 00:16:37 30576 [Note] InnoDB: 128 rollback segment(s) are active.
2016-03-13 00:16:37 30576 [Note] InnoDB: Waiting for purge to start
2016-03-13 00:16:37 30576 [Note] InnoDB: 5.6.21 started; log sequence number 93636989
2016-03-13 00:16:38 30576 [Note] Recovering after a crash using mysql-bin
2016-03-13 00:16:38 30576 [Note] Starting crash recovery...
2016-03-13 00:16:38 30576 [Note] Crash recovery finished.
2016-03-13 00:16:40 30576 [Note] Server hostname (bind-address): '*'; port: 3306
2016-03-13 00:16:40 30576 [Note] IPv6 is not available.
2016-03-13 00:16:40 30576 [Note]   - '0.0.0.0' resolves to '0.0.0.0';
2016-03-13 00:16:40 30576 [Note] Server socket created on IP: '0.0.0.0'.
2016-03-13 00:16:41 30576 [Note] Event Scheduler: Loaded 0 events
2016-03-13 00:16:41 30576 [Note] /alidata/server/mysql/bin/mysqld: ready for connections.
Version: '5.6.21-log'  socket: '/tmp/mysql.sock'  port: 3306  MySQL Community Server (GPL)
160313 00:16:41 mysqld_safe Number of processes running now: 0
160313 00:16:41 mysqld_safe mysqld restarted


```

但从 MySQL 的 error.log 中，我们很难找出出是哪里出了问题。去 Google 了一下，给出的答案基本是修改"/alidata/server/mysql/my.cnf":

``` bash

	innodb_buffer_pool_size = 64M
	# 默认为128M，修改成64M、32M或者8M，但实际上重启 MySQL 之后以为能运行，实际上过了一会 MySQL 还是会出现不知原因地自动关闭。

```

但是，现在注意去看其中的一行：

``` bash 
2016-03-13 00:16:37 30576 [Note] InnoDB: Database was not shutdown normally!

```

这句话说明了 MySQL 非正常关闭，MySQL 也不知道自己为什么被关闭掉了。。。于是， MySQL 觉得真奇怪，自己又重启了自己的进程开始 Crash Recoverying，如下：

``` bash 
2016-03-13 00:16:37 30576 [Note] InnoDB: Starting crash recovery.

```

接着，大家又猜想这条日志：

``` bash
2016-03-13 00:16:37 30576 [Note] InnoDB: The log sequence numbers 52296883 and 52296883 in ibdata files do not match the log sequence number 93636989 in the ib_logfiles!

```

继续 Google，发现原来就是这里捣鬼，才出现了 MySQL 再重启之后，又继续自动关闭，再重启。于是，尝试按照 StackOverflow 上面的 suggestions 去做：

``` bash
Drop your ib_log files and Put innodb_force_recovery=6 in config file and restart your mysql it will resolve

```

重启，又可以了。。。没一会，嚓，MySQL 又宕了！这次不仅宕，而且 error.log 又多了因 ** innodb_force_recovery = 6 ** 而引起的错误。


怎么办？现在我们回到下面这条日志：

``` bash 
2016-03-13 00:16:37 30576 [Note] InnoDB: Database was not shutdown normally!

```

现在我们要看看，为什么 MySQL 会不正常关闭呢！？现在我们打开top看看。下面使用 *** [htop](http://hisham.hm/htop/) *** 进行查看：




![image](/uploads/htop1.png)

一看 mysql 进程占用的 VIRT 怎么这么多！这时，你要是多刷新几下你服务器上搭建的 WordPress 网站的主页的话，会出现 mysql 内存占用率超过 50% 的情况，这时候 mysql 进程就会被 linux 内核杀死。也就出现了我们在 MySQL 的 error.log 中看到的“[Note] InnoDB: Database was not shutdown normally!” 日志了。

为了确定这个猜测是否真实，我们去看看 kernel 日志。

``` bash

# 打开系统 log 目录
cd  /var/log/

#接着查看 messages-20160313

vim messages-20160313

```

接着你会看到：

``` bash 
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [ 1598]     0  1598    61593      495   0       0             0 AliYunDun
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24853]   500 24853    59954     6188   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24855]   500 24855    59460     5555   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24856]   500 24856    60347     5472   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24859]   500 24859    59771     5529   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24860]   500 24860    59898     5773   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24861]   500 24861    59924     4836   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [24862]   500 24862    60051     5560   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [25035]     0 25035    11302       30   0       0             0 nginx
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [25037]   500 25037    17793     1220   0       0             0 nginx
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [25038]   500 25038    17881      309   0       0             0 nginx
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [ 4296]     0  4296   199625      547   0       0             0 AliHids
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [11606]   500 11606    60371     6174   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [11608]   500 11608    60058     6237   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [11609]   500 11609    59881     5386   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [11612]   500 11612    59734     5895   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [11613]   500 11613    59410     5886   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [17646]   500 17646    56833     5508   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [17647]   500 17647    59049     5495   0       0             0 php-fpm
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [30706]     0 30706     2868       76   0       0             0 mysqld_safe
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [31748]     0 31748     4346       53   0       0             0 anacron
Mar 13 03:14:50 iZ941u4kh5rZ kernel: [31809]   501 31809   204818    26845   0       0             0 mysqld
Mar 13 03:14:50 iZ941u4kh5rZ kernel: Out of memory: Kill process 31809 (mysqld) score 52 or sacrifice child
Mar 13 03:14:50 iZ941u4kh5rZ kernel: Killed process 31809, UID 501, (mysqld) total-vm:819272kB, anon-rss:103500kB, file-rss:3880kB

```

看到最后的两条：

``` bash
Mar 13 03:14:50 iZ941u4kh5rZ kernel: Out of memory: Kill process 31809 (mysqld) score 52 or sacrifice child
Mar 13 03:14:50 iZ941u4kh5rZ kernel: Killed process 31809, UID 501, (mysqld) total-vm:819272kB, anon-rss:103500kB, file-rss:3880kB

```

好了，终于找出原因了，内存不够，杀死了 mysqld 进程。

这时怎么办呢？



*** 下面是我的优化方案： ***

## 1.创建 SWAP 分区：
	
参见教程：[How do you add swap to an EC2 instance?](http://stackoverflow.com/questions/17173972/how-do-you-add-swap-to-an-ec2-instance)

	
a.逐条运行下面的命令：


``` bash
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1
	
```
	
b.将下面一行添加到 /etc/fstab,服务器重启时自动启动swap：


``` bash
	/var/swap.1 swap swap defaults 0 0

```

## 2.降低数据库 InnoDB 引擎的缓冲区大小，以及限制 MySQL 的最大连接数(max_connections)：

	在 /alidata/server/mysql/my.cnf 的 mysqld 下添加下面两句：

``` bash
# 降低 InnoDB 缓冲区大小为 64M 或者 32M
innodb_buffer_pool_size = 64M
	
#限制最大连接数为100，在服务器配置很低时可以继续降低
max_connections = 100
	
```
	
修改完重启 MySQL：service mysqld restart

注释：max_connections 的默认值是 151，可以动态更改这个值。参见：[max_connections](https://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_max_connections)
	
	
## 3.nginx 优化：

参见：[Optimizing Nginx Configuration](https://easyengine.io/tutorials/nginx/optimization/)
	
打开 "/alidata/server/nginx/conf/nginx.conf" ：
	
``` bash	
	vim /alidata/server/nginx/conf/nginx.conf 
	
```
	
优化配置如下：
	
``` bash
	worker_processes 1;
	worker_connections 256;

	
```

修改完重启 nginx：service nginx restart
	
	
如有必要，可以重启服务器: reboot
	

好了，问题解决了。

###### --- EOF ---
 
 
  
<blockquote class="blockquote-center">但行好事，莫问前程。</blockquote>
 
  

声明：转载需要注明来源地址。


参考资料：

[(分享) 阿里云服务器上 MySQL 频繁挂掉的解决方法](http://www.ipeld.net/archives/8420.html)

[Optimizing Nginx Configuration](https://easyengine.io/tutorials/nginx/optimization/)

[How do you add swap to an EC2 instance?](http://stackoverflow.com/questions/17173972/how-do-you-add-swap-to-an-ec2-instance)

[MySQL crashes randomly with “Database was not shut down normally” message in log](http://dba.stackexchange.com/questions/54200/mysql-crashes-randomly-with-database-was-not-shut-down-normally-message-in-log)







