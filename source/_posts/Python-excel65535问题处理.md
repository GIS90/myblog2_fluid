---
title: 解决Python处理Excel65535的问题
index_img: /img_index/index/20210805-001.jpg
categories:
  - [Python]
tags: [Python, Excel]
abbrlink: 26307
date: 2021-08-05 22:31:28
updated: 2021-08-05 22:31:28
desc: 解决Python处理Excel65535的问题
keywords: python, Excel, 65535, ValueError
---



#### 问题描述

使用Python去处理表格的时候，居然报错，提示为：
```
ValueError: row index was 65536, not allowed by .xls format
```




<!--more-->
<hr />


基于xlrd、xlwt进行Excel的处理，其包中对单个Sheet限制最大行数为65535，当读写数据超出这个范围就会出现如上错误。

#### 解决方案

可以使用openpyxl包，其最大行数为1048576，存储的文件类型为xlsx，但是弊端就是不能读取xls老的Excel格式文件。
对于这个，我在项目中想了一个解决方案，判断是xls格式就用xlrd、xlwt包的方法；xlsx就用openpyxl。
如果是xls并且行数超过65536，让用户上传xlsx格式的文件，网上有xls、xlsx格式的转换，有需要的可以找找。

如果使用openpyxl读取.xls文件，报错：
```
raise InvalidFileException(msg)
openpyxl.utils.exceptions.InvalidFileException: openpyxl does not support the old .xls file format,
please use xlrd to read this file, or convert it to the more recent .xlsx file format.
```
