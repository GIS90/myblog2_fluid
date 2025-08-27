---
title: Python项目API请求CORS跨域的问题
index_img: /img_index/index/20220123-001.png
categories:
  - Python
tags:
  - Python
  - Flask
desc: Python项目API请求CORS的跨域
keywords: 'Python, api, cors, 跨域'
abbrlink: 44368
date: 2022-01-23 23:22:39
updated: 2022-01-23 23:22:39
---


{% label info@Python %} {% label primary@CORS %}

{% note primary %}
记录一下前端请求后台造成***CORS***跨域的问题，本文以Python为开发语言，讲述解决跨域的问题，其他后台API语言也都类似。
{% endnote %}

<!--more-->
<hr />

之前的项目差不多都是使用Jijia2模板去搞的web服务，最近新起了一个新项目，打算采用VUE框架写前端，主要使用到了VUE-Element-Admin这个框架，是一位字节跳动的大佬开发的，具体的详情可以去学习：https://panjiachen.github.io/vue-element-admin-site/zh/
基于这个前端框架去开发自己的系统，用的Flask去写接口，启动2个项目之后，发现前端get数据失败，打开控制台去看：
```
Access to XMLHttpRequest at 'http://127.0.0.1:9999/user/info?token=098f6bcd4621d373cade4e832627b4f8' from origin 'http://localhost:10000' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: The 'Access-Control-Allow-Origin' header has a value 'http://localhost:9527' that is not equal to the supplied origin.
```
大致的错误就是CORS跨域请求的问题。

{% raw %}
<div class="post_cus_note">解决方案</div>
{% endraw %}

> 安装

```
pip install flask_cros
```

> 全局资源

```
from flask_cors import CORS, cross_origin


app = Flask(__name__)
app.config['SECRET_KEY'] = 'PythonNB'
CORS(app, supports_credentials=True)
```

> 蓝图资源

这里主要使用了蓝图。
```
from flask_cors import CORS


user = Blueprint('user', __name__, url_prefix='/user')
CORS(user, supports_credentials=True)
```


> 特定资源

在特定的方法上加上修饰器。
```
from flask_cors import cross_origin

@cross_origin(supports_credentials=True)
```

本人的前端功底比较弱，但是发现学完vue之后，不管是语法还是UI界面都是很不错的，有想学习前端的同学还是推荐学习vue的，<font size=6.5 color='red'>坚持每天学习，爱变编程。</font>
