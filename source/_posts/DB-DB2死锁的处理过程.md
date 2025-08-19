---
title: DB2死锁的处理过程
index_img: /img_index/index/20220226-001.jpg
categories:
  - [数据库]
tags: [DB2]
abbrlink: 18147
date: 2022-02-26 20:09:42
updated: 2022-02-26 20:09:42
desc: DB2死锁的处理过程
keywords: DB2死锁的处理过程
---


{% note primary %}
之前项目主要用Mysql、PostgreSQL，这两天DB2数据库发生-668代码问题，有个表锁死了，大概解决了一天，记录一下。提前说明一下，关于-668以及存在解决方案了，这个文章只是为了记录解决的一个过程，尝试各种方法吧。
{% endnote %}

{% label warning@DB2 %} {% label info@死锁 %}

<!--more-->
<hr />

学习参考：
- http://www.mirocn.com/info/details_156.html
- https://www.cnblogs.com/ruingy/p/3605487.html
- https://zhuanlan.zhihu.com/p/201010573
