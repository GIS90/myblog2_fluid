---
title: MariaDB安装与配置
index_img: /img_index/index/20201123-001.png
categories:
  - 数据库
tags:
  - Mysql
desc: 基于Centos7安装MariaDB数据库，并配置与初始化环境
keywords: 'Mysql, MariaDB, 安装, 配置, mysql_secure_installation, grants'
abbrlink: 3296
date: 2020-11-23 23:38:32
updated: 2020-11-23 23:38:32
---

{% label default@MariaDB %} {% label success@配置 %} {% label info@user %}

### 前言
{% note primary %}
在腾讯云上的服务器到期了，又重新买了一个3年的，需要把项目、数据库、管理工具都得迁移新的机器，本篇主要讲MariaDB的安装、初始化以及一些配置，后续会给出一个云服务器环境初始化的讲解。
简单说下Mysql与MariaDB的不同：MySQL创始人担心MySQL被Oracle收购后使用MySQL收费，于是基于MySQL6.0研发的分支研发了MariaDB，开源、性能、操作等与MySQL一样。
{% endnote %}


<!--more-->
<hr />

### 1、系统说明

|  Name   |   Version    |
|:-------:|:------------:|
| 服务器  | Centos7.5x64 |
| MariaDB |    5.5.68    |

### 2、正文

#### 2.1、安装
```
yum -y install mariadb mariadb-server
```
* mariadb为客户端
* mariadb-server服务端

> 安装指定版本

```
# yum 查询 && 安装
yum list | grep mariadb
yum -y install mariadb-server.x86_64

# rpm 不建议使用
rpm -qa | grep mariadb
rpm -ivh xxxxx
```
#### 2.2、开启服务
```
# 开启服务
systemctl start mariadb
# 设置为开机自启动服务
systemctl enable mariadb
```
Centos7之后，服务管理用systemctl。

#### 2.3、初始化配置
```
mysql_secure_installation
```
执行数据库初始化命令：
- Enter current password for root (enter for none):
回车就行，初始化的时候root密码默认是空的。
- Set root password? [Y/n]
是否设置root用户密码，输入y
- New password:
- Re-enter new password:
输入2次root密码，2次不一样会让重新输入。
- Remove anonymous users? [Y/n]
是否移除匿名用户，输入 y
- Disallow root login remotely? [Y/n]
是否拒绝root远程登录，建议输入y，毕竟是root，为了数据安全，为每个数据库创建指定用户，在创建一个总的用户来进行操作，后续会给出相关配置。
- Remove test database and access to it? [Y/n]
是否删除test数据库，输入y，不需要测试数据库。
- Reload privilege tables now? [Y/n]
重新加载权限表，输入y。

#### 2.4、测试
```
mysql -u root -p
```
输入密码即可。

#### 2.5、配置UTF-8字符

切换root用户，vim操作。

> /etc/my.cnf

在[mysqld]标签下添加
```
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
```

> /etc/my.cnf.d/client.cnf

在[client]标签下添加
```
default-character-set=utf8
```

> /etc/my.cnf.d/mysql-clients.cnf

在[mysql]标签下添加
```
default-character-set=utf8
```

> 重启服务

```
systemctl restart mariadb
```

> 查看字符集

```
show variables like "%character%";
```
![](2.png)

```
show variables like "%collation%";
```
![](1.png)

#### 2.6、创建用户

上面说过了，创建一个新的用户用来管理或者连接所有数据，这个账号可以作为管理账号，以后创建每一个数据库都对应创建一个用户，分配指定权限，用于数据隔离。创建用户之前在Mysql常用命令中我已经总结了，在这里直接干命令了。
连接数据库：
```
create user 'mingliang.gao'@'%' identified by '密码';
grant all privileges on *.* to 'mingliang.gao';
# 授权root访问，建议禁止
grant all privileges on *.* TO 'root'@'%' identified by '密码' WITH GRANT OPTION;
flush  privileges;
```
- 创建一个mingliang.gao的用户，%代表可以远程连接。
- 授权所有数据库*所有表对mingliang.gao用户，第一个星号为数据库，第二个为表内容，常用的就是第一个星号改成对应分配的数据以及用户。

#### 2.7、开启远程连接

云服务器都是禁用端口访问的，所以需要开启，这里用到了firewall-cmd命令，不清楚的可以baidu一下。
Root用户操作：
```
# 查看所有防火墙信息
firewall-cmd --list-all
```
```
[sudo] password for mingliang.gao:
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: dhcpv6-client ssh
  ports: 3306/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```
```
# 添加端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent

# 加载或者重启
firewall-cmd --reload
systemctl restart firewalld
```
添加完需要reload或者重启服务，在进行list-all查询，在本机或者其他机器可以进行连接了。


### 3、结束语

后续会出一篇关于云服务器初始化环境的文章。

<hr />

<font size=6.5 color='red'>既然决定了在技术道路上走下去，就要坚持，致自己。。。。。。</font>
