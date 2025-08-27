---
title: Oracle数据库ORA32001问题的解决
index_img: /img_index/index/20231102-001.jpeg
categories:
  - [数据库]
tags: [Oracle]
abbrlink: 47484
date: 2023-11-02 22:07:03
updated: 2023-11-02 22:07:03
desc: Oracle数据库ORA32001问题的解决
keywords: Oracle, 数据库, ORA32001
---





{% label info@Oracle %} {% label danger@ORA32001 %}

<!--more-->
<hr />

#### 环境信息

- Oracle 版本：11g Enterprise Edition Release 11.2.0.4.0 - 64bit
- Linux 版本：Red Hat Enterprise Linux Server release 7.6

#### 问题描述

在进行Oracle数据库备份的时候，执行DEFERRED_SEGMENT_CREATION参数报错。
备份参数DEFERRED_SEGMENT_CREATION {true | flase}【必须设置参数】
–true 当该参数值等于true的时候，我们数据库中新创建的表，并且没有insert过任何数据的情况下，采用导出时不会将该表导出， 因为在dba_segments和user_segments是查不到该表的信息，只有当我们插入记录才会在两表中生成段的信息，并能导出该表。
–flase 当此参数为false时，不存在dba_segments和user_segments中的段表也可以通过导出的方式将该表导出。
设置false导出所有表：
查询：show parameter DEFERRED_SEGMENT_CREATION;
更改：alter system set DEFERRED_SEGMENT_CREATION=false scope=both;

![](3811698934174_.pic.jpg)

#### 问题原因
数据库服务器Oracle数据库缺少spfile文件，需要新建配置文件。

#### 解决方案
1、切换oracle
su - oracle
2、执行创建spfile文件的命令
create spfile from pfile;
3、重启数据库
停止数据库：shutdown immediate
启动数据库：startup
4、查看系统是否有spfile配置文件
show parameter spfile;

#### 结束语

<font size=6.5 color='red'>坚持学习。。。。。。</font>
