---
title: Python模块之command系统命令
index_img: /img_index/index/article_python_module.jpeg
categories:
  - [Python]
tags: [Python, Python模块系列]
abbrlink: 26110
date: 2022-08-08 21:58:23
updated: 2022-08-08 21:58:23
desc: Python模块之command系统命令
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

CMD系统命令执行工具
利用subprocess内置包开发，使用了底层的Popen方法，便于参数交互，其subprocess的run、call等方法都基于Popen
非类的方式，对外方法：
- run_command：命令执行方法，有命令执行output返回
- run_command_no_output：命令执行方法，无命令执行output返回
- check_command_by_which：which xxxx

#### 设计

使用内置包subprocess的Popen方法进行开发，调用底层方法，便于参数交互
目前常用的3个方法：
    - run_command：命令执行方法，有命令执行output返回
    - run_command_no_output：命令执行方法，无命令执行output返回
    - check_command_by_which：which xxxx
对外使用的方法都基于_run_cmd，可以进行二次封装，实现其他的命令执行方法
也可以直接对Popen进行二次开发

#### 使用

```
_cmd = 'ls'
return_code, output = run_command(_cmd, shell=True, check_exit_code=False)
```
- return_code 命令执行状态码，0为执行正常，其他码为执行错误
- output 命令输出信息

```
_cmd = 'ls'
return_code = run_command_no_output(_cmd, shell=True, check_exit_code=False)
```
return_code 命令执行状态码，0为执行正常，其他码为执行错误

```
_cmd = 'ls'
return_code = check_command_by_which(_cmd)
```
return_code bool类型，True or False


#### 源码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    CMD系统命令执行工具
    利用subprocess内置包开发，使用了底层的Popen方法，便于参数交互，其subprocess的run、call等方法都基于Popen
    非类的方式，对外方法：
        - run_command：命令执行方法，有命令执行output返回
        - run_command_no_output：命令执行方法，无命令执行output返回
        - check_command_by_which：which xxxx

base_info:
    __author__ = "PyGo"
    __time__ = "2022/9/14"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __project__ = "quality-inspect"

usage:
     return_code, output = run_command(_cmd, shell=True, check_exit_code=False)
     - return_code 命令执行状态码，0为执行正常，其他码为执行错误
     - output 命令输出信息

     return_code = run_command_no_output(_cmd, shell=True, check_exit_code=False)
     - return_code 命令执行状态码，0为执行正常，其他码为执行错误

     return_code = check_command_by_which(_cmd)
     - return_code bool类型，True or False

design:
    使用内置包subprocess的Popen方法进行开发，调用底层方法，便于参数交互
    目前常用的3个方法：
        - run_command：命令执行方法，有命令执行output返回
        - run_command_no_output：命令执行方法，无命令执行output返回
        - check_command_by_which：which xxxx
    对外使用的方法都基于_run_cmd，可以进行二次封装，实现其他的命令执行方法
    也可以直接对Popen进行二次开发

reference urls:

python version:
    python3


Enjoy the good time everyday！！!
Life is short, I use python.

------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python command.py
# ------------------------------------------------------------
import sys
import os
import inspect
from subprocess import PIPE, Popen

from deploy.logger import logger as LOG
from config import DEBUG


def get_cur_folder():
    if getattr(sys, "frozen", False):
        return os.path.dirname(os.path.abspath(__file__))
    else:
        cur_folder = os.path.dirname(inspect.getfile(inspect.currentframe()))
        return os.path.abspath(cur_folder)


def _print_die(message):
    """
    exit process

    :param message: print error message
    :return:
    """
    if not message:
        return
    print('%s%s%s' % ('>' * 10, message, '<' * 10))
    sys.exit(1)


def _run_cmd(cmd, shell=True, check_exit_code=True, cwd=None):
    """
    execute command at local shell, build in method,
    not allow other file to use

    :param cmd: command
    :param shell: is or not run shell mode
        if shell is True, command type is str
        else command type is list or tuple
    :param check_exit_code: check is or not return code
    :param cwd: current work dir
    :return: command return code, command content

    cmd:
      - "ls -a"
      - ["ls", "-a"]
    """
    if type(cmd) not in [type('string'), type([1]), type((1,))]:
        return -1, 'Command type is error'

    if not cwd:
        cwd = get_cur_folder()

    # 统一转成string命令格式
    if not shell:
        cmd = ' '.join(cmd)
    if not isinstance(cmd, str):
        cmd = ' '.join(cmd)
    if DEBUG:
        LOG.debug(cmd)
    p = Popen(cmd,
              shell=False,
              bufsize=0,
              cwd=cwd,
              stdout=PIPE)
    # p.wait()    # 等待子进程完成
    output = p.communicate()[0]
    return_code = p.returncode
    if check_exit_code and return_code != 0:
        _print_die('Command "%s" failed.\n%s' % (' '.join(cmd), output))
    if return_code in [0, '0']:
        return_code = 0
    return return_code, output


def run_command(cmd, check_exit_code=True, shell=True, cwd=None):
    """
    run command return command code, output

    :param cmd: command
    :param check_exit_code: check command output exit code
    :param shell: shell mode
    :param cwd: current work dir
    :return: command code
    """
    return _run_cmd(cmd, shell=shell, check_exit_code=check_exit_code, cwd=cwd)


def run_command_no_output(cmd, check_exit_code=True, shell=True, cwd=None):
    """
    run command no return command output

    :param cmd: command
    :param check_exit_code: check command output exit code
    :param shell: shell mode
    :param cwd: current work dir
    :return: command code
    """
    return _run_cmd(cmd, shell=shell, check_exit_code=check_exit_code, cwd=cwd)[0]


def which_command(cmd):
    """
    which the server is or not have command

    :param cmd: command
    :return: bool
    """
    return True if run_command_no_output("which %s" % cmd, check_exit_code=False) == 0 \
        else False
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
