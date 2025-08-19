---
title: Linux学习之番外篇-nohup
index_img: /img_index/index/20200513-001.jpeg
categories:
  - [Linux]
tags: [Linux, Linux番外篇]
abbrlink: 23692
date: 2020-05-13 09:46:47
updated: 2020-05-13 09:46:47
desc: 总结一下nohub命令在Linux的相关操作
keywords: Linux, nohub, &
---


{% label default@Linux %} {% label primary@nohub...& %} {% label info@后台运行命令 %} {% label success@番外教程系列 %}

#### 简介
{% note danger %}
<font color="#red" size="5">用途</font>：用于linux后台执行命令，与&一起使用，nohup不挂断地运行命令（退出终端不会影响程序的运行）。
<font color="#blue" size="5">语法</font>：nohup command 参数 2>&1 &
{% endnote %}


<!--more-->
<hr />

这个就是比较实用而且没有任何参数的命令，语法糖就一种。

#### 基本
```
[root@localhost ~/py_work]#nohup /root/py_work/start.sh &
[1] 31491
[root@localhost ~/py_work]#nohup: ignoring input and appending output to ‘nohup.out’
```
- 建议命令写全路径，在命令可以写成sh脚本，也可以是直接***命令+参数***形式。
- 执行完nohup的时候，出现日志说明，会在***当前命令执行文件夹建立日志文件***。
- 终端退出的时候建议用exit。

#### 拓展
```
nohup /root/py_work/start.sh > xxxxxx.log 2>&1 &
```
2>&1说明：
将标准错误（2）重定向到标准输出（&1），标准输出（&1）再被重定向输入到日志xxxxxx.log文件中。

- 0 stdin (standard input）标准输入
- 1 stdout (standard output）标准输出
- 2 stderr (standard error）标准错误输出
- /dev/null 表示空设备文件

输出文件中的方式：

- ```>``` xxxx.log **输出**到文件
- ```>>``` xxxx.log **追加**到文件
