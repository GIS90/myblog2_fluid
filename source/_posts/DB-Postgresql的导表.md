---
title: Postgresql的导表
index_img: /img_index/index/20180905-001.png
categories:
  - [数据库]
tags: [Postgresql]
abbrlink: 57733
date: 2018-09-05 21:01:08
updated: 2019-09-05 21:00:54
desc: 关于Postgresql的数据导出方案，导表备份
keywords: Postgresql, 导表, psql, 备份, 数据
---


#### 背景
{% note primary %}
前面已经介绍了常用的备份与恢复了，接下来介绍一下<font color='red' size=4.5>导表</font>。
{% endnote %}

{% label info@Postgresql %} {% label warning@psql %} {% label danger@导表 %}

<!--more-->
<hr />

#### 正文

很多情况，会有把数据导出的需求，轻重缓急总会有特别紧急的情况，但是又不是专业干db的人，还是记录下来，以防不时之需。

针对于导表，个人总结了主要有2种方案，方案主要针对于服务器使用，具体能用Navicat等客户端能直连数据库的用户不用看了。

> 客户端

```#SQL
psql --dbname=my_db_name --host=db_host_ip --username=my_username -c "COPY (select id as COL_ID, name as COL_NAME from my_tab order by id) TO PATH with csv header"
```

> 服务器

```#SQL
COPY (select id as ID, description as TNAME from my_table order by id) to ‘path’ with csv header;
```
