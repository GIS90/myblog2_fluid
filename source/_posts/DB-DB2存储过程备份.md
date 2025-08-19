---
title: DB2存储过程备份
index_img: /img_index/index/20210420-001.png
categories:
  - 数据库
tags:
  - DB2
  - 存储过程
desc: DB2数据库的存储过程自动化定时备份
keywords: 'DB2, 数据库, 存储过程, 自动化, 定时, 备份, procedure'
abbrlink: 55299
date: 2021-04-20 23:34:08
updated: 2021-04-20 23:34:08
---


{% note primary %}
基于Windows系统的db2自动化存储过程备份，每天一次，这样再也不用担心存储过程修改之后不能复原的问题了。
主要使用到了routine这个命令。
{% endnote %}

{% label primary@DB2 %} {% label default@存储过程 %} {% label danger@自动备份 %}

<!--more-->
<hr />

项目的数据是基于DB2存储过程进行计算的，所以存储过程的修改、增加、删除是必须操作，为了防止复原进行自动化定时备份，主要分为2部分：

### 生成备份SQL

```
select 'db2 get routine into "D:\db_backup_procdures\20210507\'||procname||'.sar" from procedure '||procname
from SYSCAT.PROCEDURES
where procschema = 'PAS';
```
其中，【D:\db_backup_procdures\20210507\】为存储过程存放的路径，需要提前建立好（目前为手动备份模式）；
【PAS】为SCHEMA。
执行完毕后，会生成对应schema所有的存储过程存储SQL语句。

### 生成文件

将上一步生成的SQL复制到一个.bat文件，在第一行填写db2连接数据库语句，大致内容：
```
db2 connect to mzlpas user pas using pas

db2 get routine into "D:\db_backup_procdures\20210507\SP_APP_LOAD_CDSJY.sar" from procedure SP_APP_LOAD_CDSJY
db2 get routine into "D:\db_backup_procdures\20210507\SP_APP_LOAD_CKZH.sar" from procedure SP_APP_LOAD_CKZH
db2 get routine into "D:\db_backup_procdures\20210507\SP_APP_LOAD_DKZH.sar" from procedure SP_APP_LOAD_DKZH
...
```
把脚本放到db2cmd中进行执行，完成存储过程备份。


<font size=6.5 color='red'>自动化备份脚本完成中，未完待续。。。。。。</font>
