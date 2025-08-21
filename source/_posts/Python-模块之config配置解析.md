---
title: Python模块之config配置解析
index_img: /img_index/index/article_python_module.jpeg
categories:
  - [Python]
tags: [Python, Python模块系列]
abbrlink: 51787
date: 2022-08-31 23:15:26
updated: 2022-08-31 23:15:26
desc: Python模块之config配置解析
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

用来解析config.yaml配置文件，添加默认值。


#### 源码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    解析配置

base_info:
    __author__ = "PyGo"
    __time__ = "2022/9/14"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __project__ = "quality-inspect"

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
# usage: /usr/bin/python config.py
# ------------------------------------------------------------
import os
import sys
import yaml
import inspect
import logging


# logging.basicConfig()
logger = logging.getLogger(__name__)


# get current folder, solve is or not frozen of the script
def _get_cur_folder():
    if getattr(sys, "frozen", False):
        return os.path.dirname(os.path.abspath(__file__))
    else:
        cur_folder = os.path.dirname(inspect.getfile(inspect.currentframe()))
        return os.path.abspath(cur_folder)


# get global projects by mode
def _get_global_projects(mode='dev'):
    if mode not in ['dev', 'prod']:
        return None
    return os.path.join(os.path.join(os.path.join(_get_cur_folder(), 'etc'), mode), 'projects.json')


# get current run config by mode
def _get_config(mode='dev'):
    if mode not in ['dev', 'prod']:
        return None
    return os.path.join(os.path.join(os.path.join(_get_cur_folder(), 'etc'), mode), 'config.yaml')


# default log dir
def __get_log_dir():
    return os.path.join(_get_cur_folder(), 'log')


# default log dir
def __get_result_dir():
    return os.path.join(_get_cur_folder(), 'result')


"""
default config
"""
# SERVER
NAME = 'quality-inspect'
VERSION = 'v1.0.0'
DEBUG = True
SECRET_KEY = 'BelivemeIcanfly'

"""
enrty: initializate config
"""
mode = os.environ.get('mode') or 'dev'
_config_file = _get_config(mode)
if not os.path.exists(_config_file):
    logger.critical('====== config file is not exist, exit ======')
    sys.exit(1)

with open(_config_file, 'r', encoding='UTF-8') as f:
    _config_info = yaml.safe_load(f)
    if not _config_info:
        logger.critical('====== config file is unavail, exit ======')
        sys.exit(1)

    # SERVER
    NAME = _config_info['SERVER']['NAME'] or NAME
    VERSION = _config_info['SERVER']['VERSION'] or VERSION
    DEBUG = _config_info['SERVER']['DEBUG']
    DEBUG = False if not DEBUG else DEBUG
    SECRET_KEY = _config_info['SERVER']['SECRET_KEY'] or SECRET_KEY
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
