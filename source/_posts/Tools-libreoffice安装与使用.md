---
title: Libreoffice安装与使用
index_img: /img_index/index/20210112-001.png
categories:
  - [服务器]
tags: [Linux, Libreoffice, 办公工具]
abbrlink: 10477
date: 2021-01-12 17:21:54
updated: 2021-01-12 17:21:54
desc: Linux服务器上用于文档格式转换的工具，开源并且免费
keywords: Linux, Libreoffice, 办公操作, word, pdf, 格式转换
---


### 简介
{% note primary %}
官网介绍：
LibreOffice is Free and Open Source Software. LibreOffice is a powerful and free office suite, a successor to OpenOffice(.org), used by millions of people around the world. Its clean interface and feature-rich tools help you unleash your creativity and enhance your productivity. LibreOffice includes several applications that make it the most versatile Free and Open Source office suite on the market: Writer (word processing), Calc (spreadsheets), Impress (presentations), Draw (vector graphics and flowcharts), Base (databases), and Math (formula editing).
大致意思：LibreOffice是开源、免费的，用于文档方面处理的工具，很牛逼。
功能模块：
- 文档-Writer(word processing)
- 表格-Calc (spreadsheets)
- PPT-Impress (presentations)
- 绘图-Draw (vector graphics and flowcharts)
- 数据库-Base (databases)
- 算法-Math (formula editing)

<font color='red' size=4.5>基于Linux安装并使用</font>！！！。
{% endnote %}

{% label danger@Libreoffice %} {% label info@Linux %} {% label primary@办公工具 %}

<!--more-->
<hr />

LibreOffice跨平台，可以Windows、Linux、MacOS多平台安装，最新版为7.0.4。
下载地址：https://www.libreoffice.org/download/download/

### 版本信息

|     name     |               version                |
|:------------:|:------------------------------------:|
| 腾讯云服务器 | CentOS Linux release 7.5.1804 (Core) |
| Libreoffice  |                6.4.7                 |


{% raw %}
<div class="post_cus_note">--------------- Let's go ------------</div>
{% endraw %}

### 安装

#### 下载文件
选择 Linux x86_64(rpm) 的版本，下载得到 LibreOffice_6.4.7_Linux_x86-64_rpm.tar.gz。
Linux环境建议使用wget工具。
```
wget //download.documentfoundation.org/libreoffice/stable/6.4.7/mac/x86_64/LibreOffice_6.4.7_MacOS_x86-64.dmg
```

#### 清理环境
在安装之前，先删除已经安装的 LibreOffice:
```
yum remove libreoffice*
```

#### 解压
```
tar -xvf LibreOffice_6.4.7_Linux_x86-64_rpm.tar.gz
```
#### 安装
```
cd LibreOffice_6.4.7_Linux_x86-64_rpm/RPMS
yum localinstall *.rpm 或者 rpm -ivh *.rpm
```

#### 测试
```
[mingliang.gao@VM-0-15-centos install_packages]$ which libreoffice6.4
/usr/bin/libreoffice6.4
```
安装目录：/opt/libreoffice6.4
可以用find或者locate去查找安装目录。

#### 安装依赖

执行 libreoffice6.4 可能会提示库文件找不到libcairo.so.2，执行下面几条命令安装需要的库:
```
yum install cairo -y
yum install cups-libs -y
yum install libSM -y
```

### pdf转word

```
libreoffice6.4 --headless --infilter='writer_pdf_import' --convert-to doc:"MS Word 2007 XML" db2.pdf
```

### 其他

LibreOffice for Android and iOS
While The Document Foundation doesn’t currently offer an Android or iOS version of LibreOffice, there is a LibreOffice-based product in app stores from Collabora, one of our certified developers and ecosystem members:
暂时不支持移动端，支持了之后，感觉格式转换这好多公司要没了。。。

### 学习参考

官网：https://www.libreoffice.org/
中文官网：https://zh-cn.libreoffice.org/
官网安装说明：https://www.libreoffice.org/get-help/install-howto/
Linux安装：https://wiki.documentfoundation.org/Installing_LibreOffice_on_Linux#Fedora_.2F_CentOS

任何学习的东西在官网都找得到，对于LibreOffice本人也是初窥门径，日后也会给出其他的相关使用命令。


<font color="red" size="5">***只有不断的学习才明白自己菜得一逼，加油，继续学习，致自己！！！***</font>
