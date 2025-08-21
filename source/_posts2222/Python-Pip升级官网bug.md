---
title: Pip升级之后No module named 'pip._internal'问题解决
index_img: /img_index/index/20210621-001.jpg
categories:
  - [Python]
tags: [Python, PIP]
abbrlink: 19559
date: 2021-06-21 22:56:58
updated: 2021-06-21 22:56:58
desc: Pip升级官网bug
keywords: Python, PIP, ModuleNotFoundError, pip._internal
---


#### 问题起源

项目使用virtualenv创建虚拟环境，用来做项目的运行环境，发现创建新环境之后，使用pip install 包的时候出现一下错误: <font color="red" size="5">ModuleNotFoundError: No module named 'pip._internal'</font>。
网上查找了很久，记录一下解决的方案。

{% label primary@PIP %}

<!--more-->

<hr />

#### 解决方案

使用python对pip进行升级。
```
source .venv/bin/activate
python -m pip install --upgrade pip
```

#### 参考学习

gitub解决讨论：https://github.com/pypa/pip/issues/5373
