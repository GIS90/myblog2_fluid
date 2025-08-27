---
title: DB2导入与导出
index_img: /img_index/index/20201110-001.png
categories:
  - [数据库]
tags: [DB2, 命令工具]
abbrlink: 33542
date: 2020-11-10 22:55:24
updated: 2020-11-10 22:55:24
desc: DB2数据库的学习
keywords: 'DB2, db2, 数据库, 导入, 导出, load, import, db2look'
---


{% note primary %}
继续学习DB2，本篇主要关于导入与导出的操作。
{% endnote %}



<!--more-->
<hr />

{% label warning@DB2 %} {% label default@导入 %} {% label info@导出 %}

说明：
操作在db2cmd控制台。

#### 导入

模式说明：
一共有5种。
- CREATE：首先创建目标表和它的索引，然后将数据导入到新表中。该选项惟一支持的文件格式是 PC/IXF，还可以指定新表所在表空间的名称。
- INSERT：将导入的数据插入表中，目标表必须已经存在。
- INSERT_UPDATE：将数据插入表中，或者更新表中具有匹配主键的行。目标表必须已经存在，并且定义了一个主键。
- REPLACE：删除所有已有的数据，并将导入的数据插入到一个已有的目标表中。
- REPLACE_CREATE：如果目标表已经存在，则导入实用程序删除已有的数据，并插入新的数据，就像REPLACE选项那样；如果表结构不存在。创建。

codepage编码：
- 1386 GBK
- 1208 UTF-8

文件格式del与ixf区别：
- del格式是一个文本文件，文件按行来存储，含有回车的文本内容在del文件中会另起一行，del文件可视。
- ixf格式保存的是结构和数据，是一个二进制文件，ixf文件不可视。

load和import区别：
- import导入是以insert方式插入数据。
- import在导入数据的时候需要写入日志、load在导入数据的时候不需要写入日志。
- import在导入数据的时候，目标表是可以进行访问和操作的，load只能进行select操作。
- import在导入数据的时候，不能对索引模式进行选择，load可以。
- import在导入数据的时候，不会使被导入数据表所属的表空间处于backup pending状态，而load会，当然如果load加nonrecoverable就可以避免表空间处于pending。
- import在导入数据的时候，要进行各种约束性验证、触发器和参照完整性的约束，而load导入数据的时候只会进行唯一性检查和非法值检查，不会进行完整性检查，不会调用触发器。

> load

```
# 无行警告
db2 load from '文件全路径' of del modified by norowwarnings replace into 表名
# 设置编码
db2 load from '文件全路径' of del modified by codepage=1386 replace into 表名

# 监控载入的进展
db2 load query table 表名

# 中断加载
db2 load from '文件全路径' of del terminate into 表名
```

> import

```
# 标准
db2 import from 'XXXX.ixf' of ixf insert into table

# 批量提交，达到5w条commit一次
db2 import from 'filename' of del COMMITCOUNT 50000 insert into tabname

# 批量插入，一次插入500条
db2 import from 'filename' of del MODIFIED BY COMPOUND=500 insert into tabname

# 导入条数限制，只导入1w条
db2 import from 'filename' of del ROWCOUNT 10000 insert into tabname

# 导入指定起点数据
db2 import from 'filename' of del RESTARTCOUNT 10 insert into tabname

# 警告数据的条数限制
db2 import from 'filename' of del WARNINGCOUNT 10 insert into tabname

# 禁止发出行警告
db2 import from 'filename' of del MODIFIED BY NOROWWARNINGS insert into tabname
```

常用：
```
db2 import from 'filename' of del MODIFIED BY COMMITCOUNT 50000 COMPOUND=1000 insert into tabname
```

> db2
```
db2 –cvf xxx.sql
```

> 导入存储过程/函数

```
db2 -td@ -v -f e:\procudure.sql
```

#### 导出

> export

单个表导出数据，支持IXF，DEL或WSF。
```
# 标准语法
db2 export to 'XXXX.ixf' of ixf select * from XXXX where XXXX

# 不同字符集的导出 MODIFIED BY CODEPAGE=1386
db2 exprot to 'filename.del' of del MODIFIED BY CODEPAGE=1386 select * from XXXX where XXXX

# 时间字段格式化的 MODIFIED BY TIMESTAMPFORMAT="yyyy-mm-dd hh:mm:ss tt"
db2 exprot to 'filename.del' of del MODIFIED BY TIMESTAMPFORMAT="yyyy-mm-dd hh:mm:ss tt" select * from XXXX where XXXX
```
其中XXXX.ixf、filename.del文件的路径+名称。

> db2move

导入数据库的所有表及数据
```
db2move <数据库名> import
```

> db2look

导出表创建语句
```
db2look -d xxxpas -e -a -x -i pas -w pas123 -o e:\db2look.sql
db2look -d <数据库名> -u <用户> -e -o <脚本名称>.sql
```

#### 备份与恢复

缺点：必须断开所有连接，属于离线备份。

> 准备工作

首先，在指定存放文件备份的目前建立对应数据的文件夹。
第二，在执行下列命令，断开所有连接：
```
# 断开所有连接
db2 force application all

# 查看是否还有连着连接
db2 list applications
```
如果还有连接，请继续等待，直至到没有连接。

> 备份

```
db2 backup db <database name> [ to <dir name> ]
# demo
db2 backup db $生产库名 to $备份路径 PARALLELISM 2 COMPRESS
```
database name：表示数据库。
to dir name：表示为备份到的目录路径,为可选项,默认在当前目录下。

> 检查

```
db2ckbkp -h <database name>
```
备份的数据库全名。

> 恢复

```
db2 restore db <OLD_DB_NAME> [ from <dir name> ] taken at 20190505181334 into <NEW_DB_NAME>
```
OLD_DB_NAME：表示恢复的数据库名。
NEW_DB_NAME：新的数据库名称。
from dir name：表示为备份到的目录路径,为可选项,默认在当前目录下。
20190505181334：backup备份的时候有个时间。

> 历史备份查看

```
db2 list history backup all for 数据库
```

#### 学习参考

导入与导出：https://blog.csdn.net/reaper1022/article/details/18601973
