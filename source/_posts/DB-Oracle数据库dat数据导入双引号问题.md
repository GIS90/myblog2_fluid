---
title: Oracle数据库dat数据导入双引号问题
index_img: /img_index/index/20230319-001.jpg
categories:
  - [数据库]
tags: [Oracle]
abbrlink: 42107
date: 2023-03-19 22:59:03
updated: 023-03-19 22:59:03
desc: Oracle数据库dat数据导入双引号问题
keywords: Oracle,
---



{% note info %}
解决Oracle数据库导入dat数据出现的报错：second enclosure string not present。
{% endnote %}

<!--more-->
<hr />

### 背景

今天系统调度跑批，发现17号数据导入有问题。于是用sqlplus、sqlldr手工导入dat数据，打开.log日志有4条数据失败了，提示如下：
```

Record 36187: Rejected - Error on table T05_HIST_JRNL_FILE_INF, column SMY.
second enclosure string not present
Record 36188: Rejected - Error on table T05_HIST_JRNL_FILE_INF, column SMY.
second enclosure string not present
Record 43289: Rejected - Error on table T05_HIST_JRNL_FILE_INF, column SMY.
second enclosure string not present
Record 43290: Rejected - Error on table T05_HIST_JRNL_FILE_INF, column SMY.
second enclosure string not present

Table T05_HIST_JRNL_FILE_INF:
  54833 Rows successfully loaded.
  4 Rows not loaded due to data errors.
  0 Rows not loaded because all WHEN clauses were failed.
  0 Rows not loaded because all fields were null.
```
大概意思是引号缺失，上网查了一下引号可以没有，或者成对出现，

### 解决方案

打开报错的dat数据，搜索找到双引号并删除，重新导入就解决了。
