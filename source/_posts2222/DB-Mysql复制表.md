---
title: Mysql的表复制
index_img: /img_index/index/20200415-001.png
categories:
  - [数据库]
tags: [Mysql]
abbrlink: 34145
date: 2020-04-15 14:12:43
updated: 2020-04-15 14:12:43
desc: 关于Postgresql、Mysql等数据库的表复制
keywords: Mysql, 数据库, 复制, 数据
---

{% label default@Mysql %} {% label primary@表复制 %} {% label success@INSERT INTO %} {% label danger@SELECT * INTO %}

#### 1、背景


{% note primary %}
玩数据的都会涉及数据库，虽然Navicate带来了许多便利，但是本人还是喜欢使用sql，本篇介绍一下关于表复制的相关sql。
{% endnote %}

<!--more-->
<hr />

#### 2、正文

表复制分为表结构复制、表数据复制与整表（表结构+表数据）复制。

> 表结构

```
# 创建表结构
CREATE TABLE 新表 LIKE 旧表;

CREATE TABLE 新表 SELECT * FROM 旧表 WHERE 1=2;

SELECT * INTO 表2 FROM 表1 WHERE 1=2;

# 查看创建表的sql
SHOW CREATE TABLE 旧表;
```

> 表数据

```
SELECT * INTO 新表 FROM 旧表;

INSERT INTO 新表 SELECT * FROM 旧表;
```

> 表结构+表数据

```
CREATE TABLE 新表 SELECT * FROM 旧表;
```

{% note danger %}
<font size=5.5 color='red'>弊端：</font>
新表中没有旧表的primary key、auto_increment等属性，需要通过alter进行添加。
{% endnote %}

#### 3、建议

如果是想复制出一个一模一样的表，建议使用：
```
# 查看创建表的sql
SHOW CREATE TABLE 旧表;

INSERT INTO 新表 SELECT * FROM 旧表;
```
