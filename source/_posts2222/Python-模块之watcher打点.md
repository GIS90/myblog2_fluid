---
title: Python模块之watcher打点
index_img: /img_index/index/article_python_module.jpeg
categories:
  - [Python]
tags: [Python, Python模块系列]
abbrlink: 31232
date: 2022-08-28 21:55:22
updated: 2022-08-28 21:55:22
desc: Python模块之watcher打点
keywords: Python
---



{% raw %}
<div class="post_cus_note">Python模块系列</div>
{% endraw %}

<!--more-->
<hr />




#### 版本

|  名称  | 版本 |
|:------:|:----:|
| Python |  3   |


#### 描述

用于API计时打点，写入request表。

#### 设计

装饰器。

#### 使用

装饰器的方式使用。
```
from flask import request

@watcher(watcher_args=request)
watcher_args参数为request对象，里面包含blueprint, endpoint, method, path, header等请求参数
```

#### 源码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    watcher
    用于API计时打点，写入request表

base_info:
    __author__ = "PyGo"
    __time__ = "2022/8/26 23:02"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __blog__ = "www.pygo2.top"
    __project__ = "open2lisapi"

usage:
    from flask import request
    @watcher(watcher_args=request)
    watcher_args参数为request对象，里面包含blueprint, endpoint, method, path, header等请求参数

design:

reference urls:

python version:
    python3


Enjoy the good life everyday！！!
Life is short, I use python.

------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python watcher.py
# ------------------------------------------------------------
from datetime import datetime
from functools import wraps
from pprint import pprint

from deploy.utils.logger import logger as LOG
from deploy.utils.utils import get_rtx_id
from deploy.services.request import RequestService


# 声明一个全局RequestService对象
request_service = RequestService()
GLOBAL_NEW_ENDPOINR = [
    'dashboard.pan',
    'dashboard.pan_chart',
    'dashboard.index',
    'auth.user_list',
    'info.dict_list'
]


# method desc: API request to write database table [request]
def __add_request(request, cost, rtx=None):
    """
    API request information to insert into request table
    :param request: API request object parameters
    :param cost: API run time, unit is second
    :param rtx: request user rtx-id
        - 1.request get "X-Rtx-Id"
        - 2.no request by manual set
    """
    rtx_id = get_rtx_id(request) or rtx    # not rtx_id, no insert
    if not rtx_id:
        return False
    method = getattr(request, 'method')     # method allow only get or post
    if method and str(method).upper() not in ['GET', 'POST']:
        return False
    if hasattr(request, 'endpoint') and getattr(request, 'endpoint') in GLOBAL_NEW_ENDPOINR:
        RequestService().add_request(request=request, cost=cost, rtx=rtx_id)
    else:
        request_service.add_request(request=request, cost=cost, rtx=rtx_id)


# >>>>>>>>>>>>>>>>>>>>>>>>>>>>> API打点计时器 <<<<<<<<<<<<<<<<<<<<<<<<<<<<<
def watcher(watcher_args):
    def _watcher(fn):
        @wraps(fn)
        def _wrapper(*args, **kwargs):
            start = datetime.now()
            res = fn(*args, **kwargs)
            end = datetime.now()
            cost = round((end-start).microseconds * pow(0.1, 6), 4)   # API run time, unit is second
            if watcher_args:
                __add_request(request=watcher_args, cost=cost)  # API request to write database table [request]
            LOG.info('@Watcher [%s] is run: %s' % (getattr(watcher_args, 'endpoint') or fn.__name__, cost))
            return res

        return _wrapper
    return _watcher
```

#### 系列


[Python模块之command系统命令](http://pygo2.top/articles/26110/)
[Python模块之excel模块](http://pygo2.top/articles/19275/)
[Python模块之logger日志](http://pygo2.top/articles/5145/)
[Python模块之utils公共方法](http://pygo2.top/articles/61799/)
[Python模块之watcher打点](http://pygo2.top/articles/31232/)
[Python模块之config配置解析](http://pygo2.top/articles/51787/)
[Python模块之dtalk钉钉消息](http://pygo2.top/articles/58292/)
[Python模块之企业微信](http://pygo2.top/articles/33254/)

更多模块请参考文章TAG进行查看。

<font size=6.5 color='red'>***Python模块系列，持续更新中。。。。。。***</font>
