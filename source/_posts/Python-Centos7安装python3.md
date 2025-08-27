---
title: Centos7安装python3
index_img: /img_index/index/20210626-001.jpg
categories:
  - [Python]
tags: [Python, Linux]
abbrlink: 64746
date: 2021-06-26 22:00:33
updated: 2021-06-26 22:00:33
desc: 基于Centos7系统进行python3的安装
keywords: Centos7, python3, Linux, pip, 安装,
---
#### 简介

{% note primary %}
python2已经不维护更新了，不管是DEV开发、还是服务器运行环境，python3推广是迟早的事，学习一下在Centos7系统上安装python3。
{% endnote %}

<!--more-->

<hr />

### 环境

| id |  name  |               Version               |
| :-: | :----: | :----------------------------------: |
| 1 | Centos | CentOS Linux release 7.5.1804 (Core) |
| 2 | Python |                 3.7                 |

### 安装步骤

#### 检查

查看目前服务器上的现有版本。

```
which python
python --version
```

#### 下载

没有wget的命令的，可以装一下，很有用。

```
# wget安装
yum install wget

# 下载python3版本
wget https://www.python.org/ftp/python/3.7.11/Python-3.7.11.tgz
```

可以自由选择python3的版本

#### 解压

```
tar -xvf  Python-3.7.11.tgz
```

#### 创建编译安装目录

```
mkdir /usr/local/python3
```

#### 编译

```
cd Python-3.7.11
./configure --prefix=/usr/local/python3
make && make install
```

#### 创建软连

因为服务器已经存在一个python的版本了，所以命名为python3进行区分。

```
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

#### 测试

```
python3 -V
pip3 -V
```
