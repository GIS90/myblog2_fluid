---
title: 'Python之NLTK包[ERRNO61] Connection Refused'
index_img: /img_index/index/20201223-001.jpeg
categories:
  - Python
tags:
  - Python
  - Python包
  - ERROR集
desc: '解决MacOS安装nltk报[ERRNO61] Connection Refused的问题'
keywords: 'Python, NLTK, ,ERRNO, 61, Connection, Refused'
abbrlink: 38502
date: 2020-12-23 23:21:42
updated: 2020-12-23 23:21:42
---




<font size=6.5 color='red'>解决MacOS安装nltk报[ERRNO61] Connection Refused的问题。</font>


<!--more-->
<hr />

要解析一些DB2的存储过程，查询一些指定的词语，打算搞个脚本，用到nltk这个自然语言处理的包，安装之后，运行：
```
# GUI Downloader
import nltk
nltk.download()
```
出现上述情况，下载不了包。

{% raw %}
<div class="post_cus_note">=============解决方案=============</div>
{% endraw %}

#### 方案一

离线下载包，地址：https://github.com/nltk/nltk_data/tree/gh-pages
下载之后把文件放在/Users/用户名/nltk_data目录下，改下用户即可。

#### 方案二

在GUI Downloader上，更换Server Index地址：http://www.nltk.org/nltk_data/
可行，但是不是永久的，每次打开都得重新设置。

![](ok_nltk.png)

#### 方案三

找到nltk包的downloader文件，去更改默认下载地址。
我用的是conda建造的环境，或者用pycharm的去一级一级找就可以了。
位置：/opt/anaconda3/envs/python3.7/lib/python3.7/site-packages/nltk/downloader.py

![](update_url.png)

本人开发环境：MacOS + Pyhton3.7
