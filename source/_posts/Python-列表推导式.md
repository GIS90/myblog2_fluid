---
title: Python的列表推导式
index_img: /img_index/index/20170622-001.png
desc: Python列表推导式语法糖的基础介绍
categories:
  - [Python]
tags: [Python, Python基础篇]
keywords: python, 列表推导式, 语法, 语法糖
abbrlink: 30413
date: 2017-06-22 14:29:04
updated: 2017-06-22 14:29:04
---

列表推导式，是Python的一种独有特性之一，可以从一个列表（python数据结，熟称数组：[1, 2]）构构建出一个新的列表结果，构架过程中可以经过简单的数据处理。在2与3版本都支持。

<!--more-->

<hr />

```Python
alist = range(1, 10)
odd = [i for i in alist if i % 2 == 1]
print odd

[1, 3, 5, 7, 9]
```


### 1、格式

> 格式一

[表达式 for 变量 in 列表 if 条件]

- if 用于数据过滤
- 表达式 用于数据处理

> 格式二

[表达式if if 条件 else 表达式else for 变量 in 列表]

### 2、示例

示例列表：alist = ['abcd', 'bcd', 'cd', 'd']

> 列表所有元素转大写

```
[x.upper() for x in alist]

['ABCD', 'BCD', 'CD', 'D']
```

> 列表元素长度大于3的转大写
```
new_big_al = [x.upper() for x in alist if len(x) > 3]

['ABCD']
```
> for嵌套
```
[x + y for x in [1, 2] for y in [1, 2, 3]]

[2, 3, 4, 3, 4, 5]
```

> for嵌套 + if

```
[x + y for x in [1, 2] if x % 2 == 0 for y in [1, 2, 3] if y % 2 == 1]

[3, 5]
```
