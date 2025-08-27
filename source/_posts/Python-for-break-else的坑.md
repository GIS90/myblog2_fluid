---
title: Python的for_break_else的坑
index_img: /img_index/index/20181202-002.jpg
desc: 总结以下for_break_else语法糖在Python中使用的坑
categories:
  - [Python]
tags: [Python, Python基础篇]
keywords: python, for, break, else, 语法, 语法糖
abbrlink: 14093
date: 2018-12-02 10:52:23
---

{% label default@Python语法糖 %} {% label primary@for...else %}

### 简介
{% note primary %}
python语法糖中for/while循环中else+break的使用，别让break坑你
{% endnote %}

<!--more-->
<hr />

### 正文
​​​​在python语法糖中，大家应该经常使用for/while...else，官方是这么解释循环中else的使用的：
![引用官方原著引用官方原著](for_break_else_english.jpg)
重要知识点是划线部分，大致意思for/while正常结束走else，break跳转循环是不走else的。下面就用例子强行解释一波👇（最简单的例子，别介意）：
​先看个正常的：
![正常for_else](for_else.jpg)
break非正常结束for：​
![正常for_else](for_break_else.jpg)
可见，break可以引起for/while循环非正常结束​，经常使用for_else的小盆友，如果使用了break，一定要注意，本人就在使用中被坑了一把，调试了好久才知道。。。

### 备注
官方地址：http://book.pythontips.com/en/latest/for_-_else.html​​​​​
