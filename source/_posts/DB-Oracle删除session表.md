---
title: Oracle删除全局Session表
index_img: /img_index/index/20250916-001.png
tags:
  - Oracle
categories:
  - 数据库
keywords: 'Oracle, 数据库, SQL, Session'
abbrlink: 65101
date: 2025-09-27 21:48:11
updated: 2025-09-27 21:48:11
---


#### 问题

基于原来的存储过程上进行修改算法之后，发现在运行存储过程的时候报错了，一查看错误日志，发现是改了Session表结构，于是一删除操作报了以下的截图错误：ORA-14452:试图创建，变更或删除正在使用的临时表中的索引。
![](error.PNG)


<!--more-->
<hr />

#### 解决方案

> 第一步：清空数据

```SQL
truncate table session_tmp_dkghjcxx;
commit;
```

> 第二步：查询Session表的object_id
```SQL
select object_id from dba_objects where object_name = upper('session_tmp_dkghjcxx');
```

> 第三步：获取sid
```SQL
select * from v$lock where id1=14054154;  
```

> 第四步：通过sid获取sid，serial#
```SQL
select * from v$session where sid=1394;
```

> 第五步：杀死锁进程
```SQL
alter system kill session '1394,12102';
commit;
```

> 第六步：删除Session表
```SQL
drop table session_tmp_dkghjcxx;
```


#### 注意要点
在第三步获取多个sid的时候，把sid写入第四步的时候，每次写入一个，杀死进程也写入一个。