---
title: Pycharm新文件模板配置
index_img: /img_index/index/20220101-001.jpeg
categories:
  - 工具集
tags:
  - IDE
  - Python
desc: 介绍一下Pycharm的文件模板设置
keywords: 'IDE, Python, Pycharm, 模板, File and Code Template'
abbrlink: 10539
date: 2022-01-01 18:05:33
updated: 2022-01-01 18:05:33
---


{% note primary %}
<font color='red' size=4.5>分享一下关于Pycharm文件创建File and Code Template的配置。</font>
{% endnote %}

{% label default@Python %} {% label primary@Pycharm%}


<!--more-->
<hr />

今天元旦了，吃过饭后晚上还是写了会代码，用Pycharm写项目每次创建新文件的时候，都有一些文件初始化的默认内容，本人Pycharm File and Code Template配置：
```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:

base_info:
    __author__ = "PyGo"
    __time__ = "${DATE}"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __project__ = "${PROJECT_NAME}"

usage:

design:

reference urls:

python version:
    python3


Enjoy the good time everyday！！!
Life is short, I use python.

------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python ${NAME}.py
# ------------------------------------------------------------








if __name__ == '__main__':
    pass
```

常用的变量如下：
```
$ {PROJECT_NAME} - 项目的名称。
$ {NAME} - 在文件创建过程中在“新建文件”对话框中指定的新文件的名称。
$ {USER} - 当前用户的登录名。
$ {DATE} - 当前的系统日期。
$ {TIME} - 当前系统时间。
$ {YEAR} - 今年。
$ {MONTH} - 当月。
$ {DAY} - 当月的当天。
$ {HOUR} - 目前的小时。
$ {MINUTE} - 当前分钟。
$ {PRODUCT_NAME} - 将在其中创建文件的IDE的名称。
$ {MONTH_NAME_SHORT} - 月份名称的前3个字母。 示例：1月，2月等
$ {MONTH_NAME_FULL} - 一个月的全名。 示例：1月，2月等
```
