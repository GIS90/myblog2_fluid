---
title: 'DB2报错：SQLCODE=-668, SQLSTATE=57016'
index_img: /img_index/index/20210714-001.png
categories:
  - 数据库
tags:
  - DB2
abbrlink: 34462
date: 2021-07-14 20:55:15
updated: 2021-07-14 20:55:15
desc:
keywords: DB2, SQLCODE, 668
---



{% note primary %}
今日跑存储过程，很简单的一个存储过程，从来没包错过，居然报错了，提示<font color="red" size="5">***SQLCODE：-20244***</font>，网上查资料发现没有这个***SQLCODE***，于是一个表一个表的***SELECT***，发现其中有个表报错：SQLCODE=-668, SQLSTATE=57016
{% endnote %}

{% label info@DB2 %} {% label default@SQLCODE %}

<!--more-->
<hr />


当存储过程出现：SQLCODE=-668, SQLSTATE=57016，说明表处于“暂挂状态”，需要load + reorg处理下就可以了。
> 1.查询表的状态，DB2 CMD执行：db2 load query table pas.jxdx_dkzh_bf

![](668.png)
这个错误是：表处于"装入暂挂"状态。

> 2.执行以下对应的命令用来解除暂挂状态。

```
# Linux
db2 load from /dev/null of del terminate into pas.jxdx_dkzh_bf

# Windows
db2 load from E:/test.txt of del terminate into pas.jxdx_dkzh_bf
```
根据系统自行选择要执行的命令，Windows需要在命令指定的目录下建立一个空文件。

> 3.然后

```
db2 reorg table pas.jxdx_dkzh_bf
```

> 4.测试select语句，如果可以了就可以执行存储过程了。


{% raw %}
<div class="post_cus_note">注意要点</div>
{% endraw %}

- table后面跟的表名格式：策略.表名
