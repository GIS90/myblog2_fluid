---
title: DB2基础学习之常用命令
index_img: /img_index/index/20201107-001.png
categories:
  - 数据库
tags:
  - DB2
desc: DB2数据库的学习
keywords: 'DB2, db2, 数据库, 常用命令, select, create, user, table, database'
abbrlink: 8747
date: 2020-11-07 10:59:02
updated: 2020-11-07 10:59:02
---


{% note primary %}
工作环境的原因，第一次实际上使用到了DB2这个数据库，记录一下学习的要点与过程，都是一些比较基础的命令。
{% endnote %}

{% label danger@DB2 %} {% label default@命令 %}

<!--more-->
<hr />

### 1、基本操作

#### 1.1、cmd

在系统cmd中执行的命令。
```
# 普通用户
db2cmd

# 管理员cmd，有权限执行：db2start，db2stop
db2cmdadmin
```

#### 1.2、创建数据库

```
# SAMPLE数据库
db2sampl
db2sampl -force

# 标准数据库创建
db2 create db TEST  alias TEST using codeset gbk territory cn collate using system PAGESIZE 8 K
```

#### 1.3、删除数据库

```
db2 drop db XXXX
```

#### 1.4、查看数据库列表

```
# 数据库
db2 list db directory

# 节点
db2 list node directory
```

#### 1.5、数据库连接

```
# 连接
db2 connect to XXXX user 用户名 using 密码

# 断开（2种方式）
db2 connect reset
db2 terminate

# 断开所有
db2 force application all
```

#### 1.6、数据库添加schema

connect数据库状态
```
# 创建
db2 CREATE SCHEMA XXXX
# 设置
db2 SET SCHEMA XXXX
# 删除
db2  DROP SCHEMA XXXX RESTRICT
```

#### 1.7、授权

connect数据库状态
授权在这里说2种，都是亲身实践
{% raw %}
<div class="post_cus_note">代码方式</div>
{% endraw %}
语法糖：
```
db2 grant 权限 on database to user XXXX
```
注意：sql语句中的database是固定写法，不是数据库名称。
- dbadm：DBADM 用户对一个数据库有几乎完全的控制能力。DBADM 用户不能执行某些维护或管理任务
- CREATETAB：用户可以在数据库中创建表。 　　
- BINDADD：用户可以使用 BIND 命令在数据库中创建包。 　　
- CONNECT：用户可以连接数据库。 　　
- CREATE_NOT_FENCED：用户可以创建 unfenced 用户定义函数(UDF)。 　　
- IMPLICIT_SCHEMA：用户可以在数据库中隐式地创建模式，而不需要使用 CREATE SCHEMA 命令。
- LOAD：用户可以将数据装载进表中。 　　
- QUIESCE_CONNECT：用户可以访问处于静默(quiesced)状态的数据库。 　　
- CREATE_EXTERNAL_ROUTINE：用户可以创建供应用程序和数据库的其他用户使用的过程。
- secadm：安全性管理员

{% raw %}
<div class="post_cus_note">可视化操作方式</div>
{% endraw %}
- 打开DB2的控制中心
- 点击要进行授权的数据库
- 找到用户和组对象->数据库用户
- 双击要进行授权的用户
- 勾选权限，确定

![](db2_grant.png)

#### 1.8、缓冲池

connect数据库连接状态
```
# 创建 XXXX为名称
db2 CREATE BUFFERPOOL XXXX IMMEDIATE SIZE 250 AUTOMATIC PAGESIZE 8 K

# 查看
db2 select * from SYSCAT.BUFFERPOOLS

# 删除
db2 DROP BUFFERPOOL XXXX
```

#### 1.9、数据库参数配置

```
# 查看
db2 get db cfg

# 更新
db2 "UPDATE DB CFG USING KEY VALUE"
```
在更新的语法糖上，key为DB2数据库参数，value为参数值。


### 2、高级操作

#### 2.1、表空间

表空间类别：

- 系统表空间（SYSCATSPACE）
系统表空间又称为系统编目表空间，DB2系统编目表是DB2数据库保存所有DB2对象元数据的地方。

- 系统工具表空间（SYSTOOLSPACE）
系统工具表空间是供DB2管理工具和SQL管理例程使用的特定表空。

- 用户表空间（USERSPACE）
用户表空间也是数据库创建时自动创建的，表空间名称为USERSPACE1
数据库中用户表默认存放于这个表空间中，必须至少有一个用户表空间。

- 临时表空间（TEMPSPACE）
数据库管理器使用临时表空间在执行SQL操作时存储临时数据，例如排序，表重组，索引创建以及表链接等操作所产生的中间表都由临时表空间存储，数据库必须至少有一个临时表空间，也可以有多个。
DB2支持系统临时表空间和用户临时表空间两种类型，系统临时表空间必须存在，用户临时表空间可以有0个或多个。

- 常规表空间
常规表空间根据容量将表空间划分为常规表空间和大型表空间。


表空间管理类型：

- 系统管理表空间（SMS，System-Managed Space）
数据存储空间完全由操作系统管理；
SMS表空间的大小是可以动态增加的。

- 数据库管理表空间（DMS，Database-Managed Space）
DMS表空间由数据库管理系统（DBMS）自己管理控制；
这些容器的空间都是预先分配的且不允许修改大小的。

- DMS自动存储表空间（Automatic Storage DMS）
它是DMS存储的另外一种处理方法。
DMS需要很多的维护操作，而自动存储器则是作为一种简化的空间管理手段，

```
# 创建
db2 create large tablespace 表空间名 pagesize 页大小 managed by database using(容器类型 路径 大小) bufferpool 缓冲池名
# 示例
db2 CREATE LARGE TABLESPACE TBS_PAS PAGESIZE 16 K MANAGED BY DATABASE USING ( FILE '/home/TBS_PAS01' 1000M) BUFFERPOOL  IBMDEFAULTBP

# 删除
db2 drop tablespace 表空间名

# 增加表空间容器
db2 ALTER TABLESPACE  TBS_PAS add (FILE '/home/TBS_PAS02' 1000)

# 增加容器会涉及到表空间容器的重新平衡，可使用命令
db2 list utilities [show detail]

# 更改表空间容器
db2 ALTER TABLESPACE TBS_PAS resize (FILE '/home/TBS_PAS02' 1000M)

# 删除表空间容器
db2 ALTER TABLESPACE TBS_PAS drop (FILE '/home/TBS_PAS02')
```

#### 2.2、修改用户密码（控制台）

密码与系统用户保持一致，也可以通过更改用户密码来更改数据库用户密码
```
db2 connect to neu user db2admin using db2admin
db2 connect to neu user db2admin using db2admin new 1 confirm 1
```

> node节点操作

个人认为，node主要用于集群，如果是单节点，貌似没啥用
```
增加结点编目：db2 catalog tcpip node 结点名字 remote 结点所在ip地址 server 服务端口
查看结点编目：db2 list node directory
删除结点编目：db2 uncatalog node 结点别名

增加数据库编目：db2 catalog db 远程数据库名字 as 数据库别名 at node 结点名字
查看数据库编目：db2 list db directory
删除数据库编目：db2 uncatalog db 数据库别名
```

### 3、拓展

#### 3.1、命令编辑器

```
db2ce
```

#### 3.2、控制中心

```
db2cc
```

#### 3.3、配置助手

```
db2ca
```

#### 3.4、查看db2参数

```
db2 list command options
```

### 4、坑

{% note danger %}
1.节点名称不能包含下划线
{% endnote %}

### 5、学习参考

授权：http://blog.sina.com.cn/s/blog_626e70700102uxu0.html
DB2删除schema：<a href="https://blog.csdn.net/conglushao5834/article/details/100469210?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-3.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-3.edu_weight" target="_blank" class="block_project_a">DB2删除schema</a>
表空间：http://macsishu.com/db23000/db23400
node节点操作：http://blog.sina.com.cn/s/blog_4d887acf0102wydv.html
