---
title: Windows系统服务器之间的大文件互传
index_img: /img_index/index/20210717-001.png
categories:
  - [服务器]
tags: [文件传输]
abbrlink: 555
date: 2021-07-17 20:53:58
updated: 2021-07-17 20:53:58
desc: 总结一下两台Windows服务器之间的大文件传输方法
keywords: Windows, 服务器, 大文件, 互传, 传输
---

#### 简述
{% note warning %}
在服务器之间经常有文件要进行传输，而且很多情况都是10G以上的大文件，服务器操作系统主流为Windows、Linux、Unix，今日用到了两台Windows服务器之间进行大文件传输，总结一下，分享出来。
{% endnote %}

{% label default@服务器 %} {% label info@文件传输 %} {% label primary@Windows %}

<!--more-->
<hr />

#### 环境

| id  |      ip       |  name   | Version               |
|:---:|:-------------:|:-------:| --------------------- |
|  1  | 16.19.209.67  | Windows | Windows Server2008 R2 |
|  1  | 16.19.209.100 | Windows | Windows Server2008 R2 |

两台服务器为内网连同的两台机器。

#### 方案

> 文件共享

- 文件夹开启高级共享，右键->属性->共享->高级共享
- 使用MSTSC进行连接，快捷键WIN+R，输入MSTSC，输入相关远程电脑的连接信息
- 连接之后再另一台电脑的计算机顶部地址栏输入共享服务器的ip
- 选择共享的文件夹，输入对应的账号、密码
- 复制到本地

![](1.png)

> xcopy

Win系统自带的命令，启用CMD控制台。
```
# 格式
net use \\IP地址 密码/user:账户
# 示例
net use \\16.19.209.100 Aqwe123/user:Administrator

xcopy /e E:\BACKUP_MZLPAS\*.* \\16.19.209.100\E$\BACKUP_MZLPAS\ /y
/y 表示直接覆盖原来的文件，不需要询问
/e 表示复制文件夹下所有文件及文件夹
```
前提是这个文件夹开启共享。

> python

服务器具有python环境，利用python命令开启一个web服务，连接方进行下载。
```
# python2
python -m SimpleHTTPServer XXXX
```
其中，XXXX为端口。
