---
title: Linux学习之增强篇-nc
index_img: /img_index/index/20220525-001.png
categories:
  - [Linux]
tags: [Linux, Linux增强篇]
abbrlink: 20869
date: 2022-05-25 20:08:03
updated: 2022-05-25 20:08:03
desc: Linux学习之增强篇-nc
keywords: Linux, nc, netcat
---


### 1、简介

{% note danger %}
文件传输，而且还可以装逼的神器：nc（netcat）。
{% endnote %}

{% label default@Linux %} {% label primary@nc %}

<!--more-->
<hr />

目前这个命令主要用来实现Linux服务器之间文件传输，当然还有其他用于传输文件的命令，比如：scp、rz&&sz、rsync（主要用于文件自动化同步）等等，小文件的话用nc就挺好用，小推荐一下。

### 2、推荐指数
```
🌟🌟🌟
```

### 3、安装

```
# 查看是否有命令
which nc

# 安装
install -y nc
```

### 4、使用方法

介绍文件传输以及扫描端口2大功能。

#### 4.1、传输文件

```
# 发送端：
nc -v ip port < test.txt

#接收端：
nc -lp port > test.txt
```
- l：监听模式
- v：显示过程详情
其中，ip为接收端ip地址，port为自定义端口。

#### 4.2、扫描端口

```
nc -nv -w -l -z ip port
```
- n：扫描的目标是个ip地址
- l：监听模式
- v：显示过程详情
- w：设置超时时间
- z 表示 进行端口扫描

#### 4.3、参数

```
nc --help

usage: nc [-46DdhklnrStUuvzC] [-i interval] [-p source_port]
	  [-s source_ip_address] [-T ToS] [-w timeout] [-X proxy_version]
	  [-x proxy_address[:port]] [hostname] [port[s]]
```
参数挺多的，***man nc***查看明细。

参考菜鸟命令介绍：
```
-g<网关> 设置路由器跃程通信网关，最多可设置8个。
-G<指向器数目> 设置来源路由指向器，其数值为4的倍数。
-h 在线帮助。
-i<延迟秒数> 设置时间间隔，以便传送信息及扫描通信端口。
-l 使用监听模式，管控传入的资料。
-n 直接使用IP地址，而不通过域名服务器。
-o<输出文件> 指定文件名称，把往来传输的数据以16进制字码倾倒成该文件保存。
-p<通信端口> 设置本地主机使用的通信端口。
-r 乱数指定本地与远端主机的通信端口。
-s<来源位址> 设置本地主机送出数据包的IP地址。
-u 使用UDP传输协议。
-v 显示指令执行过程。
-w<超时秒数> 设置等待连线的时间。
-z 使用0输入/输出模式，只在扫描通信端口时使用。
```


### 5、其他

其他的功能暂时没怎么探索。

### 6、学习参考

菜鸟nc介绍：https://www.runoob.com/linux/linux-comm-nc.html
