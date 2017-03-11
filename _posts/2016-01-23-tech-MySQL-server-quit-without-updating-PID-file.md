---
layout: post
title: "阿里云ECS 错误”MySQL server quit without updating PID file”解决办法"
date: 2016-01-23 14:07:52 +0800
categories:
- Tech
tags: 
- Bug
- 阿里云
- MySQL
---


查看Mysql的error.log如下：

150816 10:31:21 InnoDB: Initializing buffer pool, size = 128.0M
InnoDB: mmap(137363456 bytes) failed; errno 12

———————————

Google之后发现是MySQL 5.6的问题:

``` bash

# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

```

参考链接：http://www.tocker.ca/2013/10/30/installing-the-latest-mysql-5-6-on-amazon-linux-using-official-repos.html

 

所以，打开MySQL会占用70%左右的内存，Linux 512M的内存秒秒钟不够用了。。。

 

——————————

第一种解决方法：

修改MySQL的my.cnf配置文件，添加或修改为以下代码：

``` bash
innodb_buffer_pool_size = 8M

```

然后，重启。。。

使用“top -M”命令查看内存使用情况，512M内存mysql峰值在20%左右。

![image](/uploads/mysql-quit-without-updating-pid.png)

问题解决！

——————————

第二种解决方法：

把MySQL 5.6更换为更低版本5.5或以下，同样可以解决问题。

——————————

第三种解决方法：

升级你的主机配置，内存加到1G，同样解决问题，我还有一台ECS是1G内存的，不会出现这种问题。

 

第四种解决方法：

如果在使用前面两种方法下还是出现问题，可以尝试使用第四种方法。

一般情况下，阿里的ECS没有设置swap分区，所以：

``` bash

1) dd if=/dev/zero of=/swapfile bs=1M count=1024
2) mkswap /swapfile
3) swapon /swapfile
4) 添加这行： /swapfile swap swap defaults 0 0 到 /etc/fstab 文件中

``` 
使用 free 命令查看交换空间，单位为KB：

# free -k
``` bash
             total       used       free     shared    buffers     cached
Mem:       3082356    2043700    1038656          0      50976    1646268
-/+ buffers/cache:     346456    2735900
Swap:      1048568          0    1048568

``` 

重启mysql,问题解决！

 

参考资料：
http://www.thegeekstuff.com/2010/08/how-to-add-swap-space/

 
http://stackoverflow.com/questions/10284532/amazon-ec2-mysql-aborting-start-because-innodb-mmap-x-bytes-failed-errno-12

``` bash
__转载请注明来源：http://vinci.link/bugs/%E9%98%BF%E9%87%8C%E4%BA%91ecs-%E9%94%99%E8%AF%AFmysql-server-quit-without-updating-pid-file%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/

``` 
 

