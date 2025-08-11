---
title: Mysql备份与恢复
index_img: /img_index/index/20201214-001.png
categories:
  - 数据库
tags:
  - Mysql
abbrlink: 37328
date: 2020-12-14 20:46:59
updated: 2020-12-14 20:46:59
desc: Mysql数据库的备份与恢复，Mysqldump的基础使用与参数说明
keywords: Mysql, 数据库, 备份, 恢复, mysqldump
---



{% note primary %}
服务器16号到期，得抓紧时间迁移数据库，Mysql备份主要用的是Mysqldump，学习并记录一下Mysqldump命令。
{% endnote %}



<!--more-->
<hr />

{% label default@Mysql %} {% label primary@Mysqldump %} {% label success@备份 %} {% label info@恢复 %}

<hr />

{% raw %}

<div class="post_cus_note">==========Mysqldump==========</div>
{% endraw %}

Mysqldump备份的数据为.sql格式文件，可以用vim等编辑器打开，可以查看里面的内容，都是sql语句。

### 1、备份

#### 1.1、语法糖

```
mysqldump -h [db host] -P [db port] -u [db user] -p [db name] > [file name].sql
```
- db host：数据库连接ip，如果是本机可省略不写，本机可以默认127.0.0.1。
- db port：端口，默认3306。
- -p：输入命令后，会让输入密码。
- db user：数据库用户，必填。
- db name：数据库名称。
- file name：导出的sql文件（建议：绝对路径 + 名称）。

这是最基本的语法，下面解释一下mysqldump相关参数。

#### 1.2、参数说明

介绍一下常用参数。

> 基础参数

| 参数 |         说明         |
|:----:|:--------------------:|
|  -h  |   数据库服务器host   |
|  -p  | 数据库端口，默认3306 |
|  -u  |   连接数据库的用户   |
|  -p  |    用户对应的密码    |

> -B && --databases

指定导出的数据库，有这个参数之后，导出的sql文件中会有CREATE DATABASE。
![](mysql-databases.png)


> -A --all-databases

导出全部数据库，同样导出的sql文件中也会包含创建数据库语句。

> --add-drop-database

从参数上大致可以明白，创建数据库之前先执行drop数据库语句。

> --add-drop-table

创建表之前执行drop数据表语句，默认参数，使用--skip-add-drop-table可取消。
![](mysql-add-drop-table.png)

> --add-drop-table

创建表之前执行drop数据表语句，默认参数，使用--skip-add-drop-table可取消。
![](mysql-add-drop-table.png)

> --add-locks

在数据insert插入增加LOCK TABLES，插入后UNLOCK TABLE，默认为打开状态，使用--skip-add-locks可取消，为了数据实时与安全，不建议取消。
![](mysql-add-locks.png)

> -n && --no-create-db

只导出数据，而不添加CREATE DATABASE 语句。

> -t && --no-create-info

只导出数据，而不添加CREATE TABLE 语句。

> -d && --no-data

不导出任何数据，只导出数据库表结构。

> --compact

导出更少的输出信息，可以让缩小文件体积大小。

> -c && --complete-insert

使用完整的insert语句，包含列名称。
![](mysql-complete-insert.png)

> --default-character-set

设置导出字符集，默认值为utf8。

> -single-transaction

适合Innodb事务数据库的备份，来保证备份的一致性，原理是设定本次会话的隔离级别为Repeatable read，来保证本次会话（也就是dump）时，不会看到其它会话已经提交了的数据。
如果数据库引擎是Innodb，备份语句加上这个参数。
查看数据库引擎：
```
# Mysql支持哪些存储引擎
show engines;

# Mysql当前存储引擎
show variables like '%storage_engine%';
```

> --where, -w

只导出给定的WHERE条件选择的记录，主要用于备份表。

> --set-gtid-purged=off

加了--set-gtid-purged=OFF时，在会记录binlog日志，如果不加，不记录binlog日志，所以在我们做主从用了gtid时，用mysqldump备份时就要加--set-gtid-purged=OFF，否则你在主上导入恢复了数据，主没有了binlog日志，同步则不会被同步。

> -ignore-table

不导出指定表。指定忽略多个表时，需要重复多次，每次一个表。每个表必须同时指定数据库和表名。例如：--ignore-table=database.table1 --ignore-table=database.table2

#### 1.3、常用命令

> 备份指定1个数据库

- 无--databases参数

** 建议使用 **
```
# 备份
mysqldump -h [db host] -P [db port] -u [db user] -p [db name] > [file name].sql

# 恢复
mysql -h [db host] -P [db port] -u [db user] -p [db name] < [file name].sql
```
这样导出的数据无CREARE DARABASE语句，需要提前在新的数据库提前创建好数据库，在mysql恢复的时候指定数据库。
数据进行恢复的时候，不会更改数据库原有配置，例如指定字符集之类的配置。
如果新的数据不存在会存在报错：ERROR 1046 (3D000) at line 22: No database selected。

- 有--databases参数

```
# 备份
mysqldump -h [db host] -P [db port] -u [db user] -p --add-drop-database --databases [db name] > [file name].sql

# 恢复
mysql -h [db host] -P [db port] -u [db user] -p < [file name].sql
```
加上--databases参数指定数据库之后，不需要手动创建数据，加上--add-drop-database参数，备份的sql文件中会先drop-create数据库，避免创建数据库出现问题。如果不加--add-drop-database参数，数据库已存在进行数据恢复会报错：database exist。

> 备份多个数据库

```
# 多个，在--databases后面加上导出的数据库，用空格分割
mysqldump -h [db host] -P [db port] -u [db user] -p --add-drop-database --databases [db name1] [db name2] > [file name].sql

# 全部
mysqldump -h [db host] -P [db port] -u [db user] -p --add-drop-database --all-databases > [file name].sql
```
数据库备份不建议多个或者全部，在恢复的时候会把备份的数据都进行恢复，导致没问题的数据库也会进行恢复。不过应该有办法解决恢复多个数据库问题，还没查，日后会补上。

> 压缩命令来压缩备份文件

** 建议使用 **
```
# 备份
mysqldump -h [db host] -P [db port] -u [db user] -p [db name] | gzip > [file name].sql.gz

# 恢复
gunzip < [file name].sql.gz | mysql -h [db host] -P [db port] -u [db user] -p [db name]
```
缩小体积，加快传输效率。

> 备份指定表

```
mysqldump -h [db host] -P [db port] -u [db user] -p [db name] [table name] > [file name].sql
```
如果是多个表，写在table name后面，用空格分割。

> 表数据或者表结构

这2个命令放到一起，便于记忆，但是针对于备份数据库来说，用的不是很多。
```
# 全部表结构备份
mysqldump -h [db host] -P [db port] -u [db user] -p --no-data [db name] > [file name].sql

# 全部表数据备份
mysqldump -h [db host] -P [db port] -u [db user] -p --no-create-info [db name]  > [file name].sql

# 指定表结构备份
mysqldump -h [db host] -P [db port] -u [db user] -p --no-data [db name] [table name] > [file name].sql

# 指定表数据备份
mysqldump -h [db host] -P [db port] -u [db user] -p --no-create-info [db name]  [table name] > [file name].sql
```
* -d  = --no-data
* -t  = --no-create-info

{% raw %}
<div class="post_cus_note">==========Mysql==========</div>
{% endraw %}

### 2、恢复


> 标准

```
mysql -h [db host] -P [db port] -u [db user] -p < [file name].sql
```

> 指定数据库

```
mysql -h [db host] -P [db port] -u [db user] -p [db name] < [file name].sql
```


> 压缩文件

```
gunzip < [file name].sql.gz | mysql -h [db host] -P [db port] -u [db user] -p [db name]
```



{% raw %}
<div class="post_cus_note">学习参考</div>
{% endraw %}

Mysqldump官网：https://dev.mysql.com/doc/refman/5.6/en/mysqldump.html
