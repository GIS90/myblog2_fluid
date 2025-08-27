---
title: Flask请求参数的总结
index_img: /img_index/index/20220211-001.jpg
categories:
  - Python
tags:
  - Python
  - Flask
desc: 总结一下Flask框架接收请求参数的所有方法
abbrlink: 40970
date: 2022-02-11 15:22:56
updated: 2022-02-11 15:22:56
keywords: Python, Flask, args, values, form, data, get_json, files, headers
---



{% label danger@总结一下Flask框架接收请求参数的所有方法 %}

<!--more-->

<hr />

日常写项目用Flask比较多，不管是写成前端集成的还是单独作为API项目性能都还是很不错的，对Flask API接收参数做一个总结，记录一下。
首先，引用flask对象的request。

#### GET请求
```
from flask import request

@app.route('/user/info', methods=['GET'])
@cross_origin(supports_credentials=True)
def info():
    print(request.args)
    print(request.values)
```

> requests.args

平常开发用这个获取get参数比较多，数据类型：
```
ImmutableMultiDict([('rtx', 'mingliang.gao')])
```

> request.values

也可以用这个获取参数，但是values同时获取get以及post请求的参数，数据类型：
```
CombinedMultiDict([ImmutableMultiDict([('rtx', 'mingliang.gao')]), ImmutableMultiDict([])])
```

#### POST请求
```
from flask import request

@app.route('/user/info', methods=['GET'])
@cross_origin(supports_credentials=True)
def info():
    print(request.form)
    print(request.data)
    print(request.get_json())
```

> requests.form

主要是对于前端form表单的参数获取，数据类型：
```
ImmutableMultiDict([])
```

> request.data

json数据的获取方式之一，但是数据是byte类型，不建议使用。

> request.get_json()

json数据最常用的方式，直接就是dict类型。

#### 文件
```
from flask import request

@app.route('/file/', methods=['GET'])
@cross_origin(supports_credentials=True)
def file():
    print(request.files)
```

> requests.files

用files去获取请求的文件，数据流是二进制文件，stream写入文件。


#### 其他

> requests.method

用来获取当前请求的方法，get、post等。

> requests.url

请求的URL地址。

> requests.headers

http请求的headers内容，可以直接用get方法进行数据获取，例如：request.headers.get('rtx-id')。

> requests.blueprint

获取请求的蓝图。

> requests.endpoint

请求方法的endpoint。
