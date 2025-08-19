---
title: 'Python内置函数:any与all'
index_img: /img_index/index/20170506-001.jpg
desc: 介绍一下python的内置函数:any与all
categories:
  - [Python]
tags: [Python, Python基础篇]
keywords: python, any, all, 内置方法, 内置函数, 语法, 语法糖, 脚本, 程序
abbrlink: 59977
date: 2017-05-06 20:34:44
updated: 2017-05-06 20:34:44
---

### 1、总结

{% note warning %}
any()：有‘真’为True，全‘假’为False，iterable为空是False
all()：有‘假’为False，全‘真’为True，iterable为空是True
{% endnote %}

<!--more-->
<hr />

### 2、正文

这两个函数的参数都是iterable，也就是list或者tuple，接下来分别详细介绍一下。

#### 2.1、any

![any](any.png)

- iterable的任何元素只要有一个不为False、0、''，返回True；否则元素都为False、0、''或全为空，则any(iterable)为False。也就是说只有所有的iterable都为'假'，则any(iterable)为False。
- 当iterable为空的时候，函数返回值为False
- 代码
![any_demo](any_demo.png)
#### 2.2、all

![all](all.png)

- iterable的所有元素都不为False、''、0或者iterable为空，则all(iterable)为True，也就是说只要iterable元素有一个为"假"，则为False。
- 当iterable为空的时候，函数返回值为True
- 代码
![all_demo](all_demo.png)

### 3、学习

* any：https://www.programiz.com/python-programming/methods/built-in/any
* all：https://www.programiz.com/python-programming/methods/built-in/all
