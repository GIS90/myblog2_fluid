---
title: Postgresql的备份与恢复
index_img: /img_index/index/20180902-001.png
categories:
  - [数据库]
tags: [Postgresql, 命令工具]
top: false
abbrlink: 17366
date: 2018-09-02 11:01:08
updated: 2018-09-02 11:01:08
desc: 关于Postgresql数据库的备份与恢复
keywords: Postgresql, pg_dump, 备份, 恢复, 数据库, 迁移, 数据
---
### 1、背景

{% note primary %}
身为一枚主要做后端的程序员，跟数据库打交道是必不可少的，虽然数据库由专门的dba进行负责，`<font color='red' size=4.5>`技多不压身`</font>`。于是，把日常对postgresql数据库的日常操作比较频繁的进行记录下来，一是自己找的时候方便，另外也与大家做个分享交流。
{% endnote %}

{% label default@Postgresql %} {% label primary@pg_dump %} {% label success@备份 %} {% label info@恢复 %} {% label danger@迁移 %}

<!--more-->

<hr />

### 2、命令介绍

除去写相关的sql工作，工作上用到比较多的就是数据库的备份与恢复、导表，今天先介绍一下数据库的`<font color='blue' size=4.2>`备份与恢复`</font>`。
pg_dump是一个用于备份的命令工具，即使当前数据库正在使用，也能够生成一致性的备份文件，生成sql文件或其他格式文件，且不会阻塞其他用户访问数据库(包括读、写)，下面就详细介绍一下pg_dump。

#### 2.1、 pg_dump详解

执行pg_dump --help，英文内容如下：

```
$ pg_dump --help

pg_dump dumps a database as a text file or to other formats.

Usage:
  pg_dump [OPTION]... [DBNAME]

General options:
  -f, --file=FILENAME          output file or directory name
  -F, --format=c|d|t|p         output file format (custom, directory, tar,
                               plain text (default))
  -j, --jobs=NUM               use this many parallel jobs to dump
  -v, --verbose                verbose mode
  -V, --version                output version information, then exit
  -Z, --compress=0-9           compression level for compressed formats
  --lock-wait-timeout=TIMEOUT  fail after waiting TIMEOUT for a table lock
  --no-sync                    do not wait for changes to be written safely to disk
  -?, --help                   show this help, then exit

Options controlling the output content:
  -a, --data-only              dump only the data, not the schema
  -b, --blobs                  include large objects in dump
  -B, --no-blobs               exclude large objects in dump
  -c, --clean                  clean (drop) database objects before recreating
  -C, --create                 include commands to create database in dump
  -E, --encoding=ENCODING      dump the data in encoding ENCODING
  -n, --schema=SCHEMA          dump the named schema(s) only
  -N, --exclude-schema=SCHEMA  do NOT dump the named schema(s)
  -o, --oids                   include OIDs in dump
  -O, --no-owner               skip restoration of object ownership in
                               plain-text format
  -s, --schema-only            dump only the schema, no data
  -S, --superuser=NAME         superuser user name to use in plain-text format
  -t, --table=TABLE            dump the named table(s) only
  -T, --exclude-table=TABLE    do NOT dump the named table(s)
  -x, --no-privileges          do not dump privileges (grant/revoke)
  --binary-upgrade             for use by upgrade utilities only
  --column-inserts             dump data as INSERT commands with column names
  --disable-dollar-quoting     disable dollar quoting, use SQL standard quoting
  --disable-triggers           disable triggers during data-only restore
  --enable-row-security        enable row security (dump only content user has
                               access to)
  --exclude-table-data=TABLE   do NOT dump data for the named table(s)
  --if-exists                  use IF EXISTS when dropping objects
  --inserts                    dump data as INSERT commands, rather than COPY
  --load-via-partition-root    load partitions via the root table
  --no-comments                do not dump comments
  --no-publications            do not dump publications
  --no-security-labels         do not dump security label assignments
  --no-subscriptions           do not dump subscriptions
  --no-synchronized-snapshots  do not use synchronized snapshots in parallel jobs
  --no-tablespaces             do not dump tablespace assignments
  --no-unlogged-table-data     do not dump unlogged table data
  --quote-all-identifiers      quote all identifiers, even if not key words
  --section=SECTION            dump named section (pre-data, data, or post-data)
  --serializable-deferrable    wait until the dump can run without anomalies
  --snapshot=SNAPSHOT          use given snapshot for the dump
  --strict-names               require table and/or schema include patterns to
                               match at least one entity each
  --use-set-session-authorization
                               use SET SESSION AUTHORIZATION commands instead of
                               ALTER OWNER commands to set ownership

Connection options:
  -d, --dbname=DBNAME      database to dump
  -h, --host=HOSTNAME      database server host or socket directory
  -p, --port=PORT          database server port number
  -U, --username=NAME      connect as specified database user
  -w, --no-password        never prompt for password
  -W, --password           force password prompt (should happen automatically)
  --role=ROLENAME          do SET ROLE before dump

If no database name is supplied, then the PGDATABASE environment
variable value is used.

Report bugs to <pgsql-bugs@postgresql.org>.
```

#### 2.2、参数解析

    这里介绍一下高频&&常用的参数说明，如果想深挖，建议查看官网进行查看，文章最后会给出官网链接。

    > -f

    输出文件或目录名

    > -F && --format=c|d|t|p

    输出文件格式 (定制、目录、tar)、明文 (默认值))

    > -j, --jobs=NUM

    多任务并行

    > -Z, --compress=0-9

    被压缩格式的压缩级别

    > -a, --data-only

    只转储数据,不包括模式

    > -c, --clean

    在重新创建之前，先删除数据库对象，默认这个参数是False，如果不进行删除，可以使用drop进行手动删除已存在的数据库

    > -C, --create

    备份文件中有create database数据库的sql语句，默认是进行create

    > -E, --encoding=ENCODING

    转储以ENCODING形式编码的数据，默认是数据库编码，不常用

    > -s, --schema-only

    只备份数据库策略, 不包括数据

    > -t, --table=TABLE

    只备份指定数据库指定名称的表

    > --inserts

    以inserts命令，而不是copy命令的形式转储数据，pg_dump默认是copy模式

    > --column-inserts

    以带有列名的INSERT命令形式转储数据

    > -T, --exclude-table=TABLE

    备份的数据中将排除指定名称的表

    > --exclude-table-data=TABLE

    不转储指定名称的表中的数据

    > --if-exists

    当删除对象时使用IF EXISTS，这个会安全一点，但是时间会延长，不建议使用

#### 2.3、联接选项

    数据库连接参数是必填参数，参数与psql一致。

    > -d, --dbname=DBNAME

    数据库
    > -h, --host=主机名

    数据库服务器的主机名或IP
    > -p, --port=端口号

    数据库的端口号，默认5432
    > -U, --username=名字

    用户名
    > -w, --no-password

    永远不提示输入口令
    > -W, --password

    强制口令提示 (自动)

### 3、备份

登陆数据库服务器／远程连接的服务器，使用命令pg_dump，具体参数请查看上面相关详细介绍，下面列出几种常见的情景：

> data + schema

导出整个指定的数据库。

pg_dump -h XXXX -p XXXX -U XXXX 数据库名称 > ~/数据库.sql

pg_dump -h XXXX -p XXXX -U XXXX -d XXXX -F t > /home/q/XXXX.tar

> schema

导出整个指定数据库的策略。

pg_dump -h XXXX -p XXXX -U XXXX -s 数据库名称 > ~/数据库.sql

> data

导出整个指定数据库的表数据。

pg_dump -h XXXX -p XXXX -U XXXX -a 数据库名称 > ~/数据库.sql

> table

导出整个指定数据库的指定表。

pg_dump -h XXXX -p XXXX -U XXXX -t 表名 数据库名称 > ~/表.sql

> exclude table

导出整个指定数据库的指定表，并排除掉指定的表。

pg_dump -h XXXX -p XXXX -U XXXX -T 数据库名称 > ~/数据库.sql

pg_dump -h XXXX -p XXXX -U XXXX --exclude-table-data 表名 数据库名称 > ~/数据库.sql

### 4、迁移数据

在这里介绍3种传输方式：

- python -m SimpleHTTPServer 8888
- scp
- nc

命令的具体不做详细介绍。

### 5、恢复

#### 5.1、 切换用户

登陆到需要进行恢复的数据库服务器，切换postgres用户：

```
sudo -i
su postgres
```

#### 5.2、psql

执行psql，进行客户端。
新建数据库专属用户
create user 用户名 with password '密码';
新建数据库
create database 数据库名 owner 用户名;
分配权限
grant all privileges on database 数据库 to 用户;
退出psql，执行：psql 数据库名 <数据库.sql
加访问权限，执行vim pg_hba.conf 加入：host    数据库           用户           all                     md5

#### 5.3、恢复

psql -U XXXX -d XXXX -f XXXX.sql

### 6、问题集

{% note danger %}
1 数据库无hstore数据类型
解决：
create extension hstore;
Select pg_terminate_backend(pid) from pg_stat_activity where datname = '数据库名称'

2 aborting because of server version mismatch

服务端 & 客户端 版本不一致导致。
解决：

> 方案一

降级服务端pg版本 <= 客户端
卸载old版本，更新新版本 >= 服务端版本

> 方案二

版本共存
1.sudo yumdownloader postgresql
2.yum install perl-ExtUtils-Embed
3.sudo yum install uuid
4.sudo yum install libxslt
5.rpm -ivh postgresql rpm包
6.在 /export 软连接/home/q/export下建一个目录 pg100_data ，用户和组都给 postgres
7./opt/pg10/bin/pg_ctl -D /export/pg100_data initdb
8.postgresql.conf 复制到 /export/pg100_data 目录中
9./opt/pg10/bin/pg_ctl -D /export/pg100_data start

{% endnote %}

### 7、学习参考

pg_dump官网：https://www.postgresql.org/docs/9.2/app-pgdump.html
