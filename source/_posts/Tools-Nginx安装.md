---
title: Nginx源码安装
index_img: /img_index/index/20200520-001.jpeg
categories:
    - [工具集]
tags: [Nginx, Web服务器]
abbrlink: 47364
date: 2020-05-20 20:30:22
updated: 2020-05-20 20:30:22
desc: 源码包的Nginx安装与配置
keywords: Nginx, 安装
---


{% note primary %}
身为一个Python Web开发者，当然对Nginx的使用需要了解一点，本篇对Nginx安装进行记录，<font color='red' size=4.5>基于源码安装</font>！！！。
{% endnote %}



<!--more-->
<hr />

#### 1、系统环境

腾讯云服务器：CentOS Linux release 7.5.1804 (Core)

#### 2、安装依赖

- gcc
Nginx编译需要依赖编译依赖gcc环境，如果没有gcc环境，则需要安装：
```
yum -y install gcc
```

- pcre pcre-devel
都是正则表达库。
```
yum install -y pcre pcre-devel
```

- zlib
压缩和解压缩库。
```
yum install -y zlib zlib-devel
```

- OpenSSL 安装
ssl协议库。
```
yum install -y openssl openssl-devel
```

#### 3、下载Nginx源码包

访问下列地址，看使用哪个源码包进行下载，又开发版、稳定版、历史版本，建议稳定版。
https://nginx.org/en/download.html，
```
wget https://nginx.org/download/nginx-1.18.0.tar.gz
```
使用wget进行下载，没有的搞一下，wget可是一个下载神器。

解压
```
tar -zxvf  nginx-1.9.9.tar.gz
```

#### 4、编译安装

```
cd nginx-1.18.0

./configure

make

make install
```
如果在编译安装过程中出现错误，大部分原因都是依赖问题，yum去解决依赖问题即可。

查看安装目录
```
whereis nginx
```
执行目录：/usr/sbin/nginx
模块所在目录：/usr/lib64/nginx
配置所在目录：/etc/nginx/
默认站点目录：/usr/share/nginx/html

#### 5、启动

两种方式启动。
```
# 启动/停止/重启
systemctl start/stop/restart nginx.service
nginx -c /etc/nginx/nginx.conf
```

#### 6、其他设置

```
# 开启启动
systemctl enable nginx.service
# 重新加载配置
systemctl reload nginx.service
# 查看状态
systemctl status nginx.service
```

#### 7、进程

- 查看进程
```
ps -ef | grep nginx
```
- 杀死
```
kill 进程ID
```

#### 8、常用命令

```
# 指定配置文件
nginx -c /etc/nginx/nginx.conf

# 重新加载配置文件，执行这个可以不用重启
nginx -s reload
```
#### 9、开放端口

如果是云服务器，需要开放指定端口。
```
# 添加端口
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 重加载或者重启
firewall-cmd --reload
systemctl restart firewalld

# 查看
firewall-cmd --list-all
```
这里根据自己的项目需求，对外开放端口。

#### 10、访问

配置完把nginx重启之后，访问IP:PORT。

![](nginx.png)
