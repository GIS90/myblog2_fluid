---
title: Python之pip包管理
index_img: /img_index/index/20171224-001.png
categories:
  - [Python]
tags: [Python, PIP]
abbrlink: 54341
date: 2017-12-24 10:58:52
updated: 2018-03-24 10:58:52
desc: 介绍一下关于pip的相关操作，pip主要用于python环境的包管理
keywords: Python, Packages, pip, install, update, 安装, 包, 管理
---



### 1、简介
{% note primary %}
{% code %}
pip is the package installer for Python.
You can use pip to install packages from the Python Package Index and other indexes.
{% endcode %}
官网说明：pip是Python包管理工具，主要用于包的查找、下载、安装、卸载的功能。
当前版本：<font size=6.5 color='red'>V19.3.1</font>
{% endnote %}

<!--more-->

### 2、安装

本人主要使用MacOX、Centos7系统，关于安装，这里介绍2种方式。

> curl

```
# 下载安装脚本
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

# 运行安装脚本
sudo python get-pip.py
```

> easy_install

```
easy_install pip
```

### 3、参数说明
```
[root@bash ~]# pip --help

Usage:
  pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
  check                       Verify installed packages have compatible dependencies.
  config                      Manage local and global configuration.
  search                      Search PyPI for packages.
  wheel                       Build wheels from your requirements.
  hash                        Compute hashes of package archives.
  completion                  A helper command used for command completion.
  debug                       Show information useful for debugging.
  help                        Show help for commands.

General Options:
  -h, --help                  Show help.
  --isolated                  Run pip in an isolated mode, ignoring environment variables and user configuration.
  -v, --verbose               Give more output. Option is additive, and can be used up to 3 times.
  -V, --version               Show version and exit.
  -q, --quiet                 Give less output. Option is additive, and can be used up to 3 times (corresponding to WARNING, ERROR, and CRITICAL logging levels).
  --log <path>                Path to a verbose appending log.
  --proxy <proxy>             Specify a proxy in the form [user:passwd@]proxy.server:port.
  --retries <retries>         Maximum number of retries each connection should attempt (default 3 times).
  --timeout <sec>             Set the socket timeout (default 90.0 seconds).
  --exists-action <action>    Default action when a path already exists: (s)witch, (i)gnore, (w)ipe, (b)ackup, (a)bort.
  --trusted-host <hostname>   Mark this host as trusted, even though it does not have valid or any HTTPS.
  --cert <path>               Path to alternate CA bundle.
  --client-cert <path>        Path to SSL client certificate, a single file containing the private key and the certificate in PEM format.
  --cache-dir <dir>           Store the cache data in <dir>.
  --no-cache-dir              Disable the cache.
  --disable-pip-version-check
                              Don't periodically check PyPI to determine whether a new version of pip is available for download. Implied with --no-index.
  --no-color                  Suppress colored output
```
参数说明略，在下面有具体使用。

### 4、基础使用

基于上面的详细参数，列举一下常用的命令。

#### 4.1、安装包

> 基础安装

使用此命令，默认安装源的最新版本。
```
pip install numpy
```

> 安装指定版本

```
pip install numpy==1.14.0
```

> 大于指定版本

```
pip install numpy>=1.14.0
```

> requirements文件安装

requirements.txt为指定的路径文件。
```
pip install -r requirements.txt
```

#### 4.2、卸载

```
pip uninstall numpy
```

#### 4.3、升级包

直接升级指定的包。

```
pip install -U numpy

pip install --upgrade numpy
```

#### 4.4、查看已安装的包

> 包==版本

执行这个命令，可以获取项目能run的包相关信息，把这些信息直接写到***requirements.txt***文件，方便项目迁移以及在其他环境运行。
```
[root@bash ~]# pip freeze

alembic==0.9.9
altgraph==0.10.2
amqp==2.2.2
aniso8601==1.2.1
anyjson==0.3.3
appnope==0.1.0
asn1crypto==0.24.0
aspy.yaml==1.1.1
```

> 包以及版本

```
[root@bash ~]# pip list

Package                                Version
-------------------------------------- -------------
alembic                                0.9.9
altgraph                               0.10.2
amqp                                   2.2.2
aniso8601                              1.2.1
anyjson                                0.3.3
appnope                                0.1.0
asn1crypto                             0.24.0
aspy.yaml                              1.1.1
```

> 查看指定包信息

```
[root@bash ~]# pip show numpy

Name: numpy
Version: 1.8.0rc1
Summary: NumPy: array processing for numbers, strings, records, and objects.
Home-page: http://www.numpy.org
Author: NumPy Developers
Author-email: numpy-discussion@scipy.org
License: BSD
Location: /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python
Requires:
Required-by: wordcloud, matplotlib
```

#### 4.5、搜索

```
pip search numpy
```

#### 4.6、查看版本

```
[root@bash ~]# pip --version

pip 19.2.3 from /Library/Python/2.7/site-packages/pip (python 2.7)
```

### 5、配置

> 建立pip配置文件

```
mkdir ~/.pip
cd ~/.pip
touch pip.conf
```

> 编辑pip配置文件

```
vim ~/.pip/pip.conf
```

本人配置文件：

```
[global]
index-url=http://pypi.douban.com/simple
timeout=90
disable-pip-version-check=true
retries=3
cache-dir=~/.pip/cache/
download-cache=~/.pip/cache
ignore-installed=true
no-dependencies=yes

[install]
trusted-host = pypi.douban.com

[list]
format=columns
```

### 6、常用命令

> 安装指定源指定包

```
pip install dateutil -i http://pypi.python.org/simple --trusted-host pypi.python.org
```

> 安装下载wheel包

whl包文件下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/

```
python -m pip install Pillow-4.0.0-cp27-cp27m-win_amd64.whl
```

### 7、学习参考

官网：
- pip官网：https://pypi.org/project/pip/
- whl包下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/

pip源：
- 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple
- 阿里云：http://mirrors.aliyun.com/pypi/simple/
- 豆瓣：http://pypi.douban.com/simple/
