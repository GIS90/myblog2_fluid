---
title: Oracle报错监听TNS-01155
index_img: /img_index/index/20230119-001.png
categories:
  - [数据库]
tags: [Oracle]
abbrlink: 4107
date: 2023-01-19 21:22:57
updated: 2023-01-19 21:22:57
desc: Oracle报错监听TNS-01155
keywords: Oracle, 监听, TNS-01155
---




{% label default@Oracle %} {% label info@ TNS-01155%}

<!--more-->
<hr />

### 背景

今天对线上数据库进行***expdp***备份，先lsntctl stop停止了监听，然后expdp进行数据库备份，备份完之后在启动监听发现直接报：TNS-01155的错误。
![](lsnrctl_start_error.PNG)

### 解决方案：

找到Oracle数据库的listener.ora文件中，vim删除以下内容代码：
![](listener.PNG)

### 启动监听

![](lsnrctl_start.PNG)
