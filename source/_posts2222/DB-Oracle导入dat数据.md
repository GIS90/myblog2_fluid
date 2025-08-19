---
title: Oracle手工导入dat数据
index_img: /img_index/index/20220918-001.jpg
categories:
  - [数据库]
tags: [Oracle]
abbrlink: 30542
date: 2022-09-18 23:33:04
updated: 2022-09-18 23:33:04
desc: Oracle手工导入dat数据
keywords: Oracle, sqlplus, sqlldr
---



{% note info %}
最近开始搞Oracle数据库，看存储过程学习了一下Oracle怎么导入dat数据的，这里记录一下，方便日后查询使用。
{% endnote %}

{% label info@Oracle %} {% label primary@sqlplus %} {% label default@sqlldr %}

<!--more-->
<hr />

#### 第一步：SQL语句

> 第一步：生成ctl文件

```
/u01/app/oracle/product/11.2.0/dbhome_1/bin/sqlplus pas/pas @/home/oracle/pas_control/control.sql T01_CPCT_BSC_INF_FILE_H T01_CPCT_BSC_INF_FILE_H.dat /data/gtp/20220830/
```

- /u01/app/oracle/product/11.2.0/dbhome_1/bin/sqlplus：为sqlplus命令的绝对路径，根据环境不同进行更改
- pas/pas：账户/密码
- @/home/oracle/pas_control/control.sql：下面会有control.sql文件详解，这里也是绝对路径
- T01_CPCT_BSC_INF_FILE_H：表名，control.sql文件会用到列举表字段信息
- T01_CPCT_BSC_INF_FILE_H.dat：dat文件名称
- /data/gtp/20220830/：dat数据存放的绝对路径

> 第二步：根据生成的ctl导入数据

```
/u01/app/oracle/product/11.2.0/dbhome_1/bin/sqlldr userid=pas/pas control=/data/gtp/20220830/T01_CPCT_BSC_INF_FILE_H.ctl log=/data/gtp/20220830/T01_CPCT_BSC_INF_FILE_H.log direct=true
```
- /u01/app/oracle/product/11.2.0/dbhome_1/bin/sqlldr：sqlldr命令的绝对路径
- userid=pas/pas：账户/密码
- control=/data/gtp/20220830/T01_CPCT_BSC_INF_FILE_H.ctl：执行第一步sqlplus会生成新的ctl文件，存放的导入命令
- log=/data/gtp/20220830/T01_CPCT_BSC_INF_FILE_H.log：导入日志
- direct=true：是否为直接路径

sqlldr是Oracle数据加载工具，还有bad、data、discard、skip等很多参数，详细查看man命令或者官网。


#### 第二步：control.sql

目的产生新的ctl文件，导入数据时候使用。

```
set echo off
set heading off
set verify off
set feedback off
set show off
set trim off
set pages 0
set concat on
set lines 300
set trimspool on
set trimout on
spool &3&1..ctl

select 'LOAD DATA'||chr (10)||
       'CHARACTERSET ZHS16GBK'||chr (10)||
       'INFILE ''&3&2'''||chr (10)||
       'APPEND INTO TABLE '||table_name||chr (10)||
       'FIELDS TERMINATED BY "" optionally enclosed by ''"'''||chr (10)||
       'TRAILING NULLCOLS'||chr (10)||'('
from   USER_TABLES
where  table_name = upper ('&1');

select decode (column_id, 1, '   ', ' , ')||
       rpad (column_name, 33, ' ')||
       decode (data_type,
               'VARCHAR2', 'CHAR(2000) NULLIF ('||column_name||'=BLANKS)',
               'FLOAT',    'DECIMAL EXTERNAL NULLIF('||column_name||'=BLANKS)',
               'NUMBER',   decode (data_precision, 0,
                           'INTEGER EXTERNAL NULLIF ('||column_name||
                           '=BLANKS)', decode (data_scale, 0,
                           'INTEGER EXTERNAL NULLIF ('||
                           column_name||'=BLANKS)',
                           'DECIMAL EXTERNAL NULLIF ('||
                           column_name||'=BLANKS)')),
               'DATE',     'DATE "MM/DD/YY"  NULLIF ('||column_name||'=BLANKS)',
               null)
from   user_tab_columns
where  table_name = upper ('&1')
order  by column_id;
select ')'
from sys.dual;
spool off
exit
```
