---
title: Python-判断PC是否连网
index_img: /img_index/index/20191231-001.jpg
categories:
  - Python
tags:
  [Python, Python基础篇, Python程序篇]
abbrlink: 19515
date: 2019-12-31 10:34:17
updated: 2019-12-31 10:34:17
desc: 利用Python的socket去测试PC是否连网，socket是开发必备技能之一
keywords: python, socket, curl, wget, 连网
---
{% label default@Python %} {% label primary@socket %} {% label success@测试PC是否连网 %}

<hr />
{% label info@Python脚本 %} {% label warning@curl %} {% label danger@wget %}

{% raw %}

<div class="post_cus_note">life is short, me use Python.</div>
{% endraw %}

2019年的最后一片技术博客了，写一个初始学Python的知识点，在这里先祝<font size=6.5 color='red'>【大家元旦快乐】`</font>`。

<!--more-->

<hr />

代码很简单，需要设置能访问的url即可。

```
import socket

def is_connected(url):

try:
    host = socket.gethostbyname(url)
    s = socket.create_connection((host, 80), 2)
    return True
except Exception as e:
    return False
URL = "www.baidu.com"

print is_connected(URL )
```

<font size=6.5 color='red'>此代码写于2016-09-12。。。。。。【自学Python期间】</font>

{% raw %}

<div class="post_cus_note">其他方式</div>
{% endraw %}

> curl

```
curl http://www.baidu.com
```

> wget

```
wget http://www.baidu.com
```

<font size=6.5 color='blue'>生命不息，奋斗不止！</font>
