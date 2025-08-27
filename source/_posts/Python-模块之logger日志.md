---
title: Python模块之logger日志
index_img: /img_index/index/article_python_module.jpeg
categories:
  - [Python]
tags: [Python, Python模块系列]
abbrlink: 5145
date: 2022-08-18 22:20:32
updated: 2022-08-18 22:20:32
desc: Python模块之logger日志
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

日志模块。

#### 使用

```
from app.utils.logger import logger

logger.debug('message')
logger.info('message')
logger.warning('message')
logger.error('message')
logger.critical('message')
```

#### 源码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    日志

    record  system information
    level: debug, info, warning, error, critical

    CRITICAL 50
    ERROR 40
    WARNING 30
    INFO 20
    DEBUG 10

formatter：
    %(levelno)s：打印日志级别的数值。

    %(levelname)s：打印日志级别的名称。

    %(pathname)s：打印当前执行程序的路径，其实就是sys.argv[0]。

    %(filename)s：打印当前执行程序名。

    %(funcName)s：打印日志的当前函数。

    %(lineno)d：打印日志的当前行号。

    %(asctime)s：打印日志的时间。

    %(thread)d：打印线程ID。

    %(threadName)s：打印线程名称。

    %(process)d：打印进程ID。

    %(processName)s：打印线程名称。

    %(module)s：打印模块名称。

    %(message)s：打印日志信息。

base_info:
    __author__ = "PyGo"
    __time__ = "2022/9/14"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __project__ = "quality-inspect"

usage:
    from app.utils.logger import logger

    logger.debug('message')
    logger.info('message')
    logger.warning('message')
    logger.error('message')
    logger.critical('message')

design:

reference urls:

python version:
    python3


Enjoy the good time everyday！！!
Life is short, I use python.

------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python logger.py
# ------------------------------------------------------------
import datetime
import inspect
import logging
import os
import sys
from logging.handlers import RotatingFileHandler
from config import LOG_DIR, LOG_LEVEL, LOG_FORMATTER, LOG_FILENAME_PREFIX

LEVEL = {
    'debug': logging.DEBUG,
    'info': logging.INFO,
    'warning': logging.WARNING,
    'error': logging.ERROR,
    'critical': logging.CRITICAL
}


logger = logging.getLogger(__name__)


# get current folder, solve is or not frozen of the script
def _get_cur_folder():
    if getattr(sys, "frozen", False):
        return os.path.dirname(os.path.abspath(__file__))
    else:
        cur_folder = os.path.dirname(inspect.getfile(inspect.currentframe()))
        return os.path.abspath(cur_folder)


def _get_now(format="%Y-%m-%d %H:%M:%S"):
    return datetime.datetime.strftime(datetime.datetime.now(), format)


# default log dir
def __get_log_dir():
    return os.path.join(_get_cur_folder(), 'log')


logdir = LOG_DIR
level = LOG_LEVEL
formatter = LOG_FORMATTER
filename_prefix = LOG_FILENAME_PREFIX


if not logdir:
    logdir = __get_log_dir()
if not os.path.exists(logdir):
    try:
        os.makedirs(logdir)
        logger.critical('====== log dir is not exist, create: %s ======' % logdir)
    except:
        logger.critical('====== log dir is not exist and create failure, exist: %s ======' % logdir)
        sys.exit(1)
if not level:
    level = 'debug'
# 格式
if not formatter:
    formatter = '%(asctime)s - %(pathname)s[line:%(lineno)d] - %(levelname)s - %(message)s'
formatter = logging.Formatter(formatter,
                              datefmt='%Y/%m/%d %H:%M:%S')

log_level = LEVEL.get(level)
logger.setLevel(level=log_level)


# 定义一个RotatingFileHandler，最多备份10个日志文件，每个日志文件最大10M
log_name = filename_prefix + '_' + _get_now(format="%Y-%m-%d") \
    if filename_prefix and filename_prefix != '-' else _get_now(format="%Y-%m-%d")
log_file = os.path.join(logdir, (log_name + '.log'))
file_handler = RotatingFileHandler(log_file,
                                   mode='a',
                                   maxBytes=10*1024*1024,
                                   backupCount=10,
                                   encoding='utf-8')
file_handler.setLevel(log_level)
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
# 控制台
stream_handler = logging.StreamHandler()
stream_handler.setLevel(log_level)
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)
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
