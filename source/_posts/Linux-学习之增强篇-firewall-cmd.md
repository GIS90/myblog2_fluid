---
title: 'Linux-学习之增强篇:firewall-cmd'
index_img: /img_index/index/20200328-001.jpg
categories: [Linux]
tags: [Linux, Linux增强篇]
desc: 记录Linux命令学习基础篇之lsof
keywords: 'linux, firewall, 服务器, 命令, 防火墙, centos7, shell, bash'
abbrlink: 60013
date: 2020-03-28 09:16:18
updated: 2020-03-28 09:16:18
---

### 1、简介

{% note danger %}
CentOS7开始，默认使用firewall来配置防火墙，没有安装iptables（旧版默认安装）。
<font color="#dd0000" size="5">时代在更新、CentOS也在更新，我们也必须要更新。</font>
{% endnote %}

{% label info@Linux %} {% label warning@防火墙 %} {% label success@高级教程系列 %}

<!--more-->
<hr />

防火墙没什么好说的，直接开始正餐。

#### 2、推荐指数

```
🌟🌟🌟🌟🌟
```

#### 3、安装
```
yum install firewalld
```

#### 4、使用方法

> 查看运行状态

|          command           |
|:--------------------------:|
|    firewall-cmd --state    |
| systemctl status firewalld |

> 启动

|   function   |               command               |
|:------------:|:-----------------------------------:|
|   开启服务   |  systemctl start firewalld.service  |
|   关闭服务   |  systemctl stop firewalld.service   |
| 开机自动启动 | systemctl enable firewalld.service  |
| 关闭开机启动 | systemctl disable firewalld.service |

> 增加端口

| function |                             command                             |
|:--------:|:---------------------------------------------------------------:|
|   永久   | firewall-cmd --permanent --zone=public --add-port=8080-8081/tcp |
|   临时   |       firewall-cmd --zone=public --add-port=8080-8081/tcp       |

- firewall-cmd：Linux中提供的操作firewall的工具。
– zone：指定作用域。
– add-port=3306/tcp：添加的端口，格式为：端口/通讯协议。
– permanent：表示永久生效，没有此参数重启后会失效。

> 删除端口

```
firewall-cmd --zone=public --remove-port=3306/tcp --permanent
```
改变端口即可。

> 查看端口

| function |          command          |
|:--------:|:-------------------------:|
|   简略   | firewall-cmd --list-ports |
|   详情   | firewall-cmd --list-ports |


#### 5、其他知识

关于***--zone=public***，应该会有人不了解，对**--zone**的参数做个说明：

|   type   | desc                                                                                       |
|:--------:|:------------------------------------------------------------------------------------------ |
|   drop   | 任何接受的网络数据包都被丢弃，没有任何回复                                                 |
|  block   | 任何接收的网络连接都被IPv4的icmp-host-prohibited信息和IPv6的icmp6-adm-prohibited信息所拒绝 |
|  public  | 在公共区域内使用，不能相信网络内的其他计算机不会对你的计算机造成危害                       |
| external | 特别是为路由器启动了伪装功能的外部网                                                       |
|   dmz    | 用于你的非军事区内的电脑                                                                   |
|   work   | 用于工作区                                                                                 |
|   home   | 用于家庭网络                                                                               |
| internal | 用于内部网络。你可以基本相信网络内的其他计算机不会危害到你                                 |
| trusted  | 可接受所有的网络连接                                                                       |

firewalld 的默认zone是public。

详细了解firewall-cmd，请使用***firewall-cmd --help***查看。

#### 6、特殊说明

添加完端口之后，记得reload或者重启，重新加载，否则不会生效。
```
# reload
firewall-cmd --reload

# restart
#别忘记重启防火墙
systemctl restart firewalld
```

本人也是属于探索阶段，欢迎大家一起交流，有问题请在博客进行留言。
