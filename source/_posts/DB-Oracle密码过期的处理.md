---
title: Oracle密码过期的处理
index_img: /img_index/index/20220104-001.jpg
categories:
  - [数据库]
tags: [Oracle]
abbrlink: 32323
date: 2022-01-04 22:08:48
updated: 2022-01-04 22:08:48
desc: 记录一下Oracle密码过期的问题
keywords: Oracle, 密码, 过期, 账户, dba_users, dba_profiles, sqlplus
---
{% note info %}
今天下午处理了一下Oracle账户过期的问题，记录一下处理的过程，密码过期+杀死死锁。
{% endnote %}

{% label info@Oracle %} {% label primary@密码过期 %}

<!--more-->

<hr />

### 背景

4号刚休完元旦的假期下午来到项目上，发现系统的调度停止了，修改系统状态又重新了一次，发现还是报错，第一个调度都跑不过去，这记录一下，后面杀死死锁进程要考。
因为在节前psql已经提示账户密码马上过期了，需要重制密码，没有理会，所以第一时间想到了是这个原因造成的，于是打开plsq输入登录密码，已经登录不上去了，问题已经确定了，那么就针对问题处理问题。

### 解决方案

具体Oracle数据库运维没什么经验，增删改查用的还是多，但是涉及到操作了，还是要谨慎一些，问了自己的好朋友、同事，以及结合baidu，处理了这次密码过期造成的问题。不多说，看下处理过程。

#### 切换oracle用户

登录服务器，切换到oracle用户，执行***sqlplus / as sysdba***操纵，如果能顺利登录Oracle数据库，直接下下个阶段。
我这的Oracle数据库部署的有问题，执行sqlplus发现提示一些错误，具体的错误就不写了，反正就是当时这个Oracle环境遍历没有配置好。打开/home/oracle/.bash_profile环境变量文件，发现以及配置如下内容：

```
umask 022
ORACLE_HOSTNAME=aespas
ORACLE_BASE=/u01/app/oracle
ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export ORACLE_SID=aespas
PATH=$PATH:$HOME/bin:$ORACLE_HOME/bin:$ORACLE_HOME/jdk/bin

export PATH
export ORACLE_HOME
LC_ALL="en_US"
LANG="en_US"
export NLS_LANG="AMERICAN_AMERICA.ZHS16GBK"
export NLS_DATE_FORMAT='YYYY-MM-DD HH24:MI:SS'
```

需要执行一下命令使环境变量生效：

```
source /home/oracle/.bash_profile
```

#### 修改密码

- 登录Oracle

```
sqlplus / as sysdba
```

- 查看指定的用户

```
select username, profile from dba_users where username = 'PAS';
```

- 查看用户指定profile密码有效期设置

```
select * from dba_profiles s where s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';
```

- 密码有效期由默认的180天修改成“无限制”

```
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

- 重新设置密码

```
ALTER USER PAS identified by pas
```

#### 杀死死锁

- 查看哪些表锁住了

```
select b.owner,b.object_name,a.session_id,a.locked_mode
from v$locked_object a,dba_objects b
where b.object_id = a.object_id;̨̨̨̨̨
```

- 查看锁死的会话

```
select b.username,b.sid,b.serial#,logon_time
from v$locked_object a,v$session b
where a.session_id = b.sid order by b.logon_time;
```

- 杀死锁死的会话

```
alter system kill session 'sid,serial';
```

其中sid、serial为上一步查询的锁死会话。

#### 运行调度

重新运行系统的调度

### 注意要点

- sqlplus登录数据库的所有sql建议手动输入，不要粘贴复制。
