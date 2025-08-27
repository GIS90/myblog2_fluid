---
title: CMD命令之Cygwin工具
index_img: /img_index/index/20210427-001.png
categories:
  - [工具集]
tags: [Linux, Shell]
abbrlink: 28873
date: 2021-04-27 21:47:02
updated: 2021-04-27 21:47:02
desc: 基于Windows系统上安装的软件，可以使用Linux命令
keywords: Linux, Shell, Cygwin
---

{% note primary %}

<font size=6.5 color='DeepPink'>Get that Linux feeling - on Windows</font>
运行于Windows系统上可以运行Linux命令的shell。
{% endnote %}



{% label warning@Linux %} {% label info@SHELL %} {% label primary@Windows %}

<!--more-->
<hr />

近5年依赖一直使用Macos系统，突然切换到WIN10多多少少还是有些不适应的，但是最不能忍受的是不能再电脑上使用命令去操作文件，很烦。。。。。。
所以上网找了下，可以在WIN上运行并且使用Linux的shell还是不少的，我看了知乎上的比较，对于cygwin的评价还是可以的，同类的还有cmder、powershell、git-for-windows等等。

![](article_cygwin.png)

#### 1、下载地址

http://www.cygwin.com/

#### 2、安装
安装：官网下载安装，目录：D:\cygwin64。

#### 3、配置

D:\cygwin64\bin
D:\cygwin64\sbin
以上路径加入到系统path，让系统cmd也可以使用命令。

#### 4、apt-cyg

> 安装

1.下载命令：https://github.com/transcode-open/apt-cyg
2.解压文件，把apt-cyg命令复制到D:\cygwin64\bin
3.重启cygwin，测试apt-cyg命令

> 常用参数

- install 安装
- remove 卸载
- update 更新
- download 下载软件但不安装
- show 展示安装包的信息
- list 列举已安装的软件
- search/searchall 查询软件

感觉跟pip、yum、brew很像啊，可能这些玩意都大概类似把。。。


#### 5、学习参考

cygwin详解：https://www.cnblogs.com/feipeng8848/p/8555648.html
apt-cyg命令详解：https://zhuanlan.zhihu.com/p/66930502
cmder神器：https://zhuanlan.zhihu.com/p/28400466
Windows命令工具大对比：https://www.zhihu.com/question/19739424


<font size=6.5 color='red'>成功没有偶然，继续努力前行</font>
