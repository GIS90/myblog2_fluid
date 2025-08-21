---
title: Python版本工具Conda常用命令
index_img: /img_index/index/20220402-001.png
categories:
  - [Python]
tags: [Python, Conda]
abbrlink: 56751
date: 2022-04-02 23:45:26
updated: 2022-04-02 23:45:26
desc: Conda常用命令
keywords: Python, Conda
---


{% label default@Python %} {% label info@Conda %}

{% note primary %}
搞Python都知道玩Python必须得有虚拟环境，那么常用的工具主要有Conda、Virtualenv、Pipenv、Pyflow。。。。。。还有其他的，常用的就是列举的那些了，经过长时间的使用，还是觉得Conda最好用，本篇记录一下Conda常用的命令，，<font color='red' size=4.5>Conda很强发大。。。。。。</font>
{% endnote %}


<!--more-->
<hr />

本篇记录Conda的常用命令，安装略。

#### 创建虚拟环境
```
# 创建环境，-n为--name的缩写参数
conda create --name env_name
conda create -n env_name

# 创建指定python3.7版本环境
conda create -n env_name python=3.7

# 创建指定python版本下包含某些包
conda create -n env_name python=3.7 XXXX XXXX XXXX

# 克隆其他环境
conda create -n new_env_name --clone old_env_name
```

#### 激活/退出
```
conda activate env_name
conda deactivate
```

#### 列举环境
```
conda info -e / conda info --envs
conda env list
```

#### 删除环境
```
conda remove -n env_name --all
```

#### 安装/卸载包
```
conda install xxx [-n env_name]
conda uninstall xxx [-n env_name]

# 激活环境
conda activate env_name
# pip安装/卸载
pip install/uninstall xxxx
```
[]括号内容为非必需填写，如果不加为当前环境。

#### 导出虚拟环境
```
# 导出
conda list -e [-n env_name] > requirements.txt
# 安装
conda install --yes --file requirements.txt
```

#### 分享虚拟环境
```
# 导出当前虚拟环境
conda env export > environment.yml

# 创建保存的虚拟环境
conda env create -f environment.yml
```

#### 帮助
```
# 总览命令帮助
conda --help

# 命令详情帮助
conda xxxx --help
```

Conda的功能非常强大，上面只列举的最、最、最常用的命令而已，想详细了解就得花点时间去学了，不过对于日常开发的话学的命令没那么多，整理完成，收工，睡觉去了。
