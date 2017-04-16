---
layout: post
title: Nginx 入门手册
date: 2017-04-15 20:42:00 +0800
categories:
- Tech
tags:
- Nginx

---

## Nginx 基本介绍

[Nginx](https://www.nginx.com/resources/wiki/)(发音同 "engine x")，是一个开源、高性能服务器，它能用于反向代理、负载均衡和HTTP缓存等。

> NGINX is a free, open-source, high-performance HTTP server and reverse proxy, as well as an IMAP/POP3 proxy server. NGINX is known for its high performance, stability, rich feature set, simple configuration, and low resource consumption. - ***Nginx Official***

----

## Nginx 安装

> 安装环境：CentOS 7.2

[安装教程](http://nginx.org/en/docs/install.html)中有两种方式，一种是通过包(package)安装，一种是通过源代码安装。

这里通过包的形式进行安装，安装教程如下：

- 直接通过 `sudo yum install nginx` 安装：[How To Install Nginx on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7) - 版本可能较旧。
- 安装最新的稳定版：[Pre-Built Packages for Stable version](http://nginx.org/en/linux_packages.html)

----

## Nginx 基本使用

- 启动：`sudo systemctl start nginx`
- 重启：`sudo systemctl restart nginx`
- 重载：`sudo systemctl reload nginx`
- 停止：`sudo systemctl stop nginx`
- 设置开机启动：`sudo systemctl enable nginx`

----

## 基本配置

配置教程：

- [nginx服务器安装及配置文件详解](https://yq.aliyun.com/articles/47359)
- [nginx做负载均衡器以及proxy缓存配置](https://segmentfault.com/a/1190000002873747?spm=5176.100239.blogcont47359.11.3jf7bT)

### Nginx 配置文件说明

配置文件路径：`/etc/nginx/nginx.conf`。

```
user  nginx;
worker_processes  1;

# 错误日志配置
error_log  /opt/home/log/nginx/error/default.log warn;
pid        /var/run/nginx.pid;


events {
    use epoll; # 事件模型，在 linux 下默认为 epoll
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /opt/home/log/nginx/access/default.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;
	 
	 # 作为默认服务器，所有通过 80 端口进来的请求，在未匹配到 server_name 时，该请求会被路由到这里。
    server {
        listen 80 default_server;
        server_name _;
        return 444; # 返回空结果
    }
    
    # 在 /etc/nginx/conf.d/ 路径下，的所有 .conf 格式的文件将被加载进来，服务器配置可以写在这个路径下。
    include /etc/nginx/conf.d/*.conf;
}

```

文件路径：`/etc/nginx/conf.d/demo.conf`。

```
server {
    listen       80;
    server_name  yourdomain.com www.yourdomain.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

### PHP 配置

```
server {
  listen 80;
  server_name yourdomain.com www.yourdomain.com;

  index index.html index.htm index.php;
  root /opt/app/yourdomain.com;
  location ~ .*\.(php|php5)?$
  {
      #fastcgi_pass  unix:/tmp/php-cgi.sock;
      fastcgi_pass  127.0.0.1:9000;
      fastcgi_index index.php;
      fastcgi_param  SCRIPT_FILENAME   $document_root$fastcgi_script_name;
      include /etc/nginx/fastcgi_params;
  }
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
  {
      expires 30d;
  }
  location ~ .*\.(js|css)?$
  {
      expires 1h;
  }
  location / {
     try_files $uri $uri/ /index.php?$args;
  }

  access_log  /opt/log/nginx/access/default.log;

}
```

### Nginx 反向代理

![A diagram of basic Reverse Proxy](https://assets.digitalocean.com/articles/high_availability/ha-diagram-animated.gif)

from [*Understanding Nginx HTTP Proxying, Load Balancing, Buffering, and Caching*](https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching)


- [NGINX REVERSE PROXY](https://www.nginx.com/resources/admin-guide/reverse-proxy/) - 较详细介绍了反向代理的各种使用方式。
- [WebSocket proxying](http://nginx.org/en/docs/http/websocket.html)
- [Understanding Nginx HTTP Proxying, Load Balancing, Buffering, and Caching](https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching)

#### 反向代理的注意点

- 请求 `Header` 中所有的空的字段不会被传递；
- Nginx 在默认配置下不会传递 `Header` 中带下划线的字段，需要进行配置后才支持 `underscores_in_headers on`；
- `Header` 的 "Host" 字段会被重写成 `$proxy_host`。

![Nginx SSL Termination](https://assets.digitalocean.com/articles/nginx_ssl_termination_load_balancing/nginx_ssl.png)

from [*How To Set Up Nginx Load Balancing with SSL Termination*](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-load-balancing-with-ssl-termination)

#### 反向代理的好处

- 统一的应用入口。可以简化登陆控制、可以使用同一个 SSL 证书(SSL Termination)来避免多证书产生的问题。
- 可以构建私有网络，来保证内部数据访问安全。
- 便于基础设施的弹性伸缩和透明维护。通过 Nginx 来切割流量的方向，来实现扩容和维护。
- URL 重写、压缩、缓存等。

### Nginx 负载均衡

![Load Balancer](https://assets.digitalocean.com/articles/architecture/load_balancer.png)

![Master-Slave Database Replication](https://assets.digitalocean.com/articles/architecture/master_slave_database_replication.png)

from [5 Common Server Setups For Your Web Application](https://www.digitalocean.com/community/tutorials/5-common-server-setups-for-your-web-application)

[Using nginx as HTTP load balancer](http://nginx.org/en/docs/http/load_balancing.html)

负载均衡的方式，主要分三种：

- **循环(round-robin)** - 请求通过循环的方式分配给各个服务器。这种当时可以给每个服务器加上不同的权重。
- **最少连接数(least-connected)** - 下一个请求将分配给当前激活连接数的最少服务器。在请求耗时较长时起到负载均衡的作用。
- **IP哈希(ip-hash)** - 根据当前请求的IP，进行哈希运算后，分配到某个服务器上。前两种负载均衡的方案不能实现来自同一个 IP 的请求都发到同一台服务器上，IP哈希的方式就可以实现。

负载均衡配置：

```
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

#### 负载均衡的权重

下面配置中，每 5 个请求会有三个被分配到 `srv1`：

```
upstream myapp1 {
        server srv1.example.com weight=3;
        server srv2.example.com;
        server srv3.example.com;
    }
```

另外，Nginx 还支持负载均衡的服务器的健康检查配置。


## Nginx 处理请求过程

Nginx 处理请求过程：[How nginx processes a request](http://nginx.org/en/docs/http/request_processing.html)

简要说明：

- 首先，发起一个 `HTTP` 请求：`http://yourdomain.com`(这个链接称为 `Host`)，通过DNS(域名解析)解析出目标主机的 IP;
- 接着，请求通过一个服务器端口(这个端口可以称为 `PORT` )，接着 `nginx` 开始处理，先通过 `Host` 匹配 `server.servar_name` ，来判断该请求要路由到哪里；
- 如果未匹配到 `server.servar_name` 或者请求中本来就没有 `Host` 的话，`nginx` 就会把这个请求路由到这个 `PORT` 对应的默认服务器(如果未通过 `listen 80 default_server` 指定默认服务器，那么第一个被加载的服务器就是默认服务器)。
- 最后，一个请求由一个 `server` 进行处理，并向浏览球返回结果。


## Nginx 调试

调试教程：[A debugging log](http://nginx.org/en/docs/debugging_log.html)

如果是通过源代码手工安装的，需要重新安装，并在安装的过程加上参数：`./configure --with-debug`

如果是通过包的形式安装(`sudo yum install nginx`)的，可以通过命令切换到调试模式：

- 第一步，停止 `nginx` 进程：`sudo systemctl stop nginx`。
- 第二步，在 `/etc/nginx/nginx.cond` 中配置 `error_log` 的配置级别为 `debug`：`error_log /your/log/path/error.log debug`。
- 第三步，开启调试模式 `sudo systemctl start nginx-debug`。
- 完成。所有的 debug 数据都会记录在 `/your/log/path/error.log` 中。

----

## Nginx 运维

Nginx 可以实现在修改配置后，无下线时间(no downtime)重启。

![load new NGINX binary with no downtime](https://cdn.wp.nginx.com/wp-content/uploads/2015/06/Screen-Shot-2015-06-08-at-12.41.51-PM.png)

from [Inside NGINX: How We Designed for Performance & Scale](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)

[基于动态策略的灰度发布系统](https://github.com/CNSRE/ABTestingGateway)


----

## Nginx 相关

- [Inside NGINX: How We Designed for Performance & Scale](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
- [The Architecture of Open Source Applications (Volume 2): nginx](http://www.aosabook.org/en/nginx.html)
- [lua-nginx-module](https://github.com/openresty/lua-nginx-module)

----

> 着装一下，去吃碗拉面，然后去 MAAN Espresso Bar，

Robin on April 16, 2017 12:52 at Wangjing CBD