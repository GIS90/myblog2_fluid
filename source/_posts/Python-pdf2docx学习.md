---
title: Python的pdf2docx模块学习
index_img: /img_index/index/20210705-001.png
categories:
  - [Python]
tags: [Python, Python程序篇]
abbrlink: 28829
date: 2021-07-05 00:44:39
updated: 2021-07-05 00:44:39
---

#### 简介
{% note default %}
利用业余时间参加了公司的创新项目活动，里面有一个PDF转WORD的功能，分享出来。主要使用了python3版本的pdf2docx包，包的使用也很简单，基于基础功能写写逻辑、封装下页面就可以做出web功能了。
{% endnote %}

{% label info@Python实战 %} {% label default@PDF转WORD %}

<!--more-->
<hr />

#### 环境

| id  |  name  | Version |
|:---:|:------:|:-------:|
|  1  | Python |   3.7   |

pdf2docx只支持python3。

#### 安装依赖

```
docx==0.2.4
fitz==0.0.1.dev2
fonttools==4.24.4
idna==2.10
lxml==4.6.3
numpy==1.21.0
opencv-python==4.5.2.54
pdf2docx==0.5.2
PyMuPDF==1.16.14
python-docx==0.8.11
```

#### 官方代码

> 示例一

转换整个文档。
```
from pdf2docx import Converter

pdf_file = '/path/to/sample.pdf'
docx_file = 'path/to/sample.docx'

# convert pdf to docx
cv = Converter(pdf_file)
cv.convert(docx_file)      # all pages by default
cv.close()
```

> 示例二

转换文档指定页。
```
# convert from the second page to the end (by default)
cv.convert(docx_file, start=1)

# convert from the first page (by default) to the third (end=3, excluded)
cv.convert(docx_file, end=3)

# convert from the second page and the third
cv.convert(docx_file, start=1, end=3)
```

> 示例三

多进程转换。
```
cv.convert(docx_file, multi_processing=True, cpu_count=4)
```

> 示例四

加密文档转换。
```
cv = Converter(pdf_file, password)
cv.convert(docx_file)
cv.close()
```

#### 参考学习

官网：https://dothinking.github.io/pdf2docx/index.html


#### 项目转换代码
```
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
--------------------------------------------------------------
describe:


base_info:
    __version__ = "v.10"
    __author__ = "mingliang.gao"
    __time__ = "2020/10/1 9:31 AM"
    __mail__ = "gaoming971366@163.com"
--------------------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python file_lib.py
# ------------------------------------------------------------
import os
from multiprocessing import cpu_count
from pdf2docx import Converter

from deploy.utils.utils import get_storefilename_by_md5, \
    get_base_dir, get_now, mk_dirs
from deploy.config import TRANSFER_BASE_DIR


class FileLib(object):
    ALLOWED_EXTENSIONS = [
        '.doc',
        '.docx',
        '.xls',
        '.xlsx',
        '.ppt',
        '.pptx',
        '.pdf',
        '.txt',
    ]

    def __init__(self):
        self.cpu_count = cpu_count()

    def allow_format_fmt(self, fname):
        return True if (os.path.splitext(fname)[1]).lower() in self.ALLOWED_EXTENSIONS else False

    def store_file(self, file, compress=False, is_md5_store_name=True):
        if not file:
            return False, {'message': '没有发现文件'}
        try:
            # 文件存储初始化
            now_date = get_now(format="%Y-%m-%d")
            real_store_dir = get_base_dir() + TRANSFER_BASE_DIR + now_date
            store_dir = os.path.join(TRANSFER_BASE_DIR + now_date)
            if not os.path.exists(real_store_dir):
                mk_dirs(real_store_dir)

            file_name = file.filename
            md5_v, store_file_name = get_storefilename_by_md5(file_name, ftype='file')
            _real_file = os.path.join(real_store_dir, store_file_name) \
                if is_md5_store_name else os.path.join(real_store_dir, file_name)
            file.save(_real_file)
            # 压缩
            # if compress:
            #     pass
            return True, {'store_name': store_file_name if is_md5_store_name else file_name,
                          'md5': md5_v,
                          'path': os.path.join(store_dir, store_file_name) if is_md5_store_name else os.path.join(store_dir, file_name),
                          'message': 'success'}
        except:
            return False, {'message': '文件存储发生错误'}

    def pdf_2_word(self, file_pdf: str, docx_name: str = None,
                   start: int = 0, end: int = None, pages: list = None,
                   is_multi_processing: bool = False, cpu_count: int = 1):
        if not file_pdf:
            return False, {'message': 'Pdf file is null', 'tar_file': ''}
        src_file = get_base_dir() + file_pdf
        if not os.path.exists(src_file):
            return False, {'message': 'Pdf file is not exist', 'tar_file': ''}
        if not os.path.isfile(src_file):
            return False, {'message': 'Pdf file is not file', 'tar_file': ''}
        if docx_name:
            docx_names = os.path.splitext(docx_name)
            if len(docx_names) < 2:
                return False, {'message': 'Pdf file name not have suffix', 'tar_file': ''}
        src_path, src_name = os.path.split(src_file)
        transfer_name_pdf = docx_name if docx_name else src_name
        transfer_name_word = os.path.splitext(transfer_name_pdf)[0] + '.docx'
        docx_file = os.path.join(src_path, transfer_name_word)
        rel_path = os.path.split(file_pdf)[0]
        tar_file = os.path.join(rel_path, transfer_name_word)
        # convert pdf to docx
        cv = Converter(src_file)
        # all pages by default
        if is_multi_processing and cpu_count >= self.cpu_count:
            cpu_count = self.cpu_count
        cv.convert(docx_file, multi_processing=True, cpu_count=cpu_count) \
            if is_multi_processing else cv.convert(docx_file)
        cv.close()
        return True, {'message': 'success', 'tar_file': tar_file}

```
