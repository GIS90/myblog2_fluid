---
title: Pycharm出现Non-Project-Files-Protection的解决方法
index_img: /img_index/index/20221215-001.png
categories:
  - 工具集
tags:
  - IDE
  - Python
desc: Pycharm出现Non-Project-Files-Protection的解决方法
keywords: 'IDE, Python, Pycharm'
abbrlink: 62345
date: 2022-12-15 23:22:37
updated: 2022-12-15 23:22:37
---



{% note primary %}
<font color='red' size=4.5>Pycharm打开Python项目出现Non Project Files Protection提示，并且处理项目文件需要确认，度娘上找了下解决方案，记录一下</font>
{% endnote %}

{% label default@Python %} {% label primary@Pycharm%}


<!--more-->
<hr />


从远程git仓库clone项目的时候，如果项目包含<font color='red' size=5.5>.idea文件夹</font>，直接用Pycharm工具打开的时候，会提示：Non Project Files Protection。
关闭Pycharm，找到项目目录，删除.idea文件夹即可。

> win

1.需要设置文件夹显示隐藏文件、文件夹
2.资源管理器删除.idea文件夹

> MacOS

```
cd 项目目录
ll
rm -rf .idea
```

Pycharm工具重新打开项目。


![](article_pycharm.jpeg)