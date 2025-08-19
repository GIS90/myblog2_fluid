---
title: Oracle数据库一次异常处理的解决经历
index_img: /img_index/index/20230803-001.jpg
categories:
  - 数据库
tags:
  - Oracle
desc: Oracle数据库一次异常处理的解决经历
keywords: Oracle
abbrlink: 29163
date: 2023-08-03 22:23:18
updated: 2023-08-03 22:23:18
---




{% note info %}
记录一次Oracle数据库挂载mount数据库失败的经历，过程中除了多次问题，在错误中寻找解决方案。
{% endnote %}

<!--more-->
<hr />

#### 背景

去了一个新项目，发现用sqlplus和sqlldr导出dat数据报错，接二连三的报了好几个，下面是解决遇到的问题与解决方案。

#### 问题一

> 问题

```
ORA-01078: failure in processingsystem parameters
LRM-00109: could not openparameterfile '/u01/app/oracle/product/11.2.0/db_1/dbs/initORA10G.ora'
```

> 解决方案

找到$ORACLE_BASE/admin/数据库名称/pfile目录下的init.ora形式的文件，复制到$ORACLE_HOME/dbs目录下，并且改成wlhtpas.ora文件名称，其中wlhtpas.ora是oracle数据库实例名ORACLE_SID。

> 原因

文件初始化spfile缺失，具体原因不详，可能是安装问题，也可能是异常断电导致。

#### 问题二

解决了之后，重新startup启动数据的时候，又有新问题。

> 问题

```
ORA-01102: cannot mount database in EXCLUSIVE mode
```

![](1.png)

查看Oracle数据库日志，发现文件被占用。

> 解决方案

1.进入/u01/app/oracle/product/11.2.0/db_1/dbs/目录
2.删除lkPOD文件

在这里有个小技巧，用lsof去查看哪些进程使用了lkPOD文件，需要kill掉。

```
lsof lkPOD
kill -9 进程ID
```

> 原因

文件被重复占用，导致启动失败。


#### 结束语

<font size=6.5 color='red'>坚持学习。。。。。。</font>
