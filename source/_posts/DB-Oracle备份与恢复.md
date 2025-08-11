---
title: Oracle备份expdp与恢复impdp
index_img: /img_index/index/20230222-001.png
categories:
  - 数据库
tags:
  - Oracle
top: false
desc: Oracle备份expdp与恢复impdp
keywords: 'Oracle, 备份, expdp, 恢复,impdp'
abbrlink: 24260
date: 2023-02-22 22:13:33
updated: 2023-02-22 22:13:33
---

记录一下关于Oracle数据库数据备份与恢复，项目上经常用，用实际项目进行总结。

<!--more-->

<hr />

{% label danger@Oracle%} {% label primary@expdp %} {% label success@impdp %} {% label info@备份 %} {% label warning@恢复 %}

#### 1、主备恢复流程摘要

1、停止【主服务器】tomcat服务(./shutdown.sh)

2、停止【主服务器】oracle监听服务（lsnrctl stop）

3、【主服务器】切换oracle用户进行expdp备份

4、主备服务器传输备份文件（MD5SUM查看是否文件完整）

5、【备服务器】impdp数据恢复

6、启动tomcat服务(./start.sh)

7、启动oracle监听服务（lsnrctl start）

#### 2、备份前数据库参数设置

> 备份参数DEFERRED_SEGMENT_CREATION {true | flase}【非必须设置参数】

–true 当该参数值等于true的时候，我们数据库中新创建的表，并且没有insert过任何数据的情况下，采用导出时不会将该表导出， 因为在dba_segments和user_segments是查不到该表的信息，只有当我们插入记录才会在两表中生成段的信息，并能导出该表。

–flase 当此参数为false时，不存在dba_segments和user_segments中的段表也可以通过导出的方式将该表导出。

查询：show parameter DEFERRED_SEGMENT_CREATION;

更改：alter system set DEFERRED_SEGMENT_CREATION=false scope=both;

﻿

> expdp备份目录【必须设置参数】

select * from dba_directories;

创建/修改：create or replace directory 备份目录名称 as '备份绝对路径';

授权：grant read,write on dirrectory 备份目录名称 to 用户;

删除：drop directory 备份目录名称;

﻿

> 普通用户授权导出全库【非必要设置参数】：

grant datadump_exp_full_database to test;

﻿

#### 3、备份命令

普通用户：expdp 账户/密码 directory=备份目录名称 dumpfile=备份文件名称.dmp logfile=备份日志名称.log compression=all reuse_dumpfiles=y

例子：
```
expdp pas/pas directory=DMPDID_PAS dumpfile=AESPAS_20230213_01.dmp logfile=AESPAS_20230213_01.log compression=all reuse_dumpfiles=y
```

system用户：expdp system/密码 directory=备份目录名称 schema=策略名称 dumpfile=备份文件名称.dmp logfile=备份日志名称.log compression=all reuse_dumpfiles=y


-- directory：指定转储文件和日志文件所在的目录（数据库设置）

-- dumpfile：备份文件的名称

-- schema：策略名称，如果system导出全库去掉schema参数，加上full=y

-- logfile：备份日志名称

-- reuse_dumpfiles：是否覆盖备份文件，Y覆盖N不覆盖

-- FULL：指定数据库模式导出,默认为N，{Y | N}为Y时,标识执行数据库导出

-- tables：导出指定的表名，%代表通配符，如果是system可以加上：策略名称.表名

-- exclude：排除指定的表名不导出

-- content：导出内容{ALL | DATA_ONLY | METADATA_ONLY}当设置CONTENT为ALL时,将导出对象定义及其所有数据.为DATA_ONLY时,只导出对象数据,为METADATA_ONLY时,只导出对象定义

-- FLASHBACK_TIME：指定导出特定时间点的表数据（FLASHBACK_TIME=“TO_TIMESTAMP(’25-08-2004 14:35:00’,’DD-MM-YYYY HH24:MI:SS’)”）

-- FILESIZE：指定导出文件的最大尺寸,默认为0

-- NOLOGFILE：该选项用于指定禁止生成导出日志文件,默认值为N

-- QUERY：用于指定过滤导出数据的where条件

-- compression：ALL ：对导出的元数据和表数据都进行压缩，得到的导出文件是最小的，耗时也是最长的。DATA_ONLY ：仅对表数据进行压缩，对于大数据量的导出效果明显，会比METADATA_ONLY方式得到更小的压缩文件。METADATA_ONLY ：仅对元数据进行压缩，而不会对表数据进行压缩，这种压缩执行后效果一般不是很明显，不过速度比较快。NONE ：不进行任何的压缩，导出后的文件也是最大的。DEFAULT ：默认方式，即不指定COMPRESSION参数，会采用默认的压缩方式METADATA_ONLY。

﻿

导出表：
expdp 账户/密码 directory=备份目录名称 dumpfile=备份文件名称.dmp logfile=备份日志名称.log compression=all reuse_dumpfiles=y tables=表名称1,表名称2,表名称N

﻿

#### 4、恢复命令
impdp 账户/密码 directory=备份目录名称 dumpfile=备份文件名称.dmp exclude=statistics table_exists_action=replace
例子：
```
impdp pas/pas directory=DMPDID_PAS dumpfile=AESPAS_20230213_01.dmp exclude=statistics table_exists_action=replace
```

#### 5、查看存在的job：

```
SELECT * FROM DBA_DATAPUMP_JOBS;
```
