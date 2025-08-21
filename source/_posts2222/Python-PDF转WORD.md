---
title: Python实现PDF转WORD的方式
index_img: /img_index/index/20220604-001.jpg
categories:
  - [Python]
tags: [Python, Python实战]
abbrlink: 11221
date: 2022-06-04 08:32:21
updated: 2022-06-04 08:32:21
desc: Python实现PDF转WORD的方式
keywords: Python, PDF转WORD, pdf2docx
---

{% label primary@Python %} {% label info@Python实战 %}

### 背景
{% note primary %}
网上的PDF转WORD都是收费的，写了好几年的Python了，打算想搞一个开源工具网站，其中的一个常用功能之一就是PDF转WORD，所以想探索一下所有的转换方式，<font color='red' size=4.5>持续更新中。。。。。。</font>
{% endnote %}


<!--more-->
<hr />

### 环境

| id  |  name  | Version |
|:---:|:------:|:-------:|
|  1  | Python |   3.7   |

### 结果比较

### 详解
记录一下所有的包，有需要的可以进行参考。

#### pdf2docx

> 官网

https://www.cnpython.com/pypi/pdf2docx#

> 代码

```
import os
from pdf2docx import Converter

data_folder = os.path.join(os.getcwd(), 'pdf')
pdf_file = os.path.join(data_folder, '阿里云数据库.pdf')
print(pdf_file)
word_file = '%s-pdf2docx.docx' % os.path.splitext(pdf_file)[0]
print(word_file)
c = Converter(pdf_file)
c.convert(word_file)
c.close()
```




#### LibreOffice

> 官网

https://www.libreoffice.org/




#### PyPDF2

> 官网

https://pypdf2.readthedocs.io/en/latest/




#### PDFMiner

> 官网

https://www.unixuser.org/~euske/python/pdfminer/programming.html




### 结束语

<font size=5.5 color='red'>***坚持每天学习。。。。。。***</font>
