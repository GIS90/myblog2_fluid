---
title: Python学习之Collections
index_img: /img_index/index/20220530-001.jpeg
categories:
  - [Python]
tags: [Python, Python包]
abbrlink: 51993
date: 2022-05-30 20:08:24
updated: 2022-05-30 20:08:24
desc: Python学习之Collections
keywords: Python, Collections
---

{% label default@Python实战 %} {% label danger@Collections %}

{% note primary %}
写代码的时候，Collections包算是常用的一个，里面的有一些数据结构还是非常好用的，本篇总结一下Collections包的常用内容，包含OrderedDict、deque、ChainMap、defaultdict、Counter等内容。
{% endnote %}


<!--more-->
<hr />



### 环境

| id  |  name  | Version |
|:---:|:------:|:-------:|
|  1  | Python |   3.7   |

### 类型

- OrderedDict：有序dict
- deque：队列
- ChainMap：链式MAP
- defaultdict：自带默认值的dict
- Counter：计数器