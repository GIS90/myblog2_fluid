---
title: DB2的表备份SQL
index_img: /img_index/index/20220516-001.jpg
categories:
  - 数据库
tags:
  - DB2
desc: DB2的表备份SQL
keywords: 'DB2, 数据库, SQL, 表备份'
abbrlink: 63086
date: 2022-05-16 21:48:11
updated: 2022-05-16 21:48:11
---

{% note primary %}
今天修改DB2的表数据CKZH进行delete，想的是对这个表先进行一个备份，于是想到create table as select * from这个的语法，发现DB2没有这个，但是有表结构+数据复制这样的模式，记录一下日后可能用得到。
{% endnote %}

{% label primary@DB2 %} {% label default@存储过程 %} {% label danger@自动备份 %}

<!--more-->
<hr />

模式：表结构 + 数据

> 表结构

```SQL
CREATE TABLE [目标表] AS (SELECT * FROM [原表]) DEFINITION ONLY;
```
直接对表进行一个复制，参数如下：
目标表：创建的目标表
原表：复制的表结构原始表

> 插入数据

```SQL
INSERT INTO JXDX_CKZH_20220516
SELECT * FROM JXDX_CKZH
-- WHERE
```
WHERE后面加的是插入数据的限制，没有限制就去掉WHERE就行。
