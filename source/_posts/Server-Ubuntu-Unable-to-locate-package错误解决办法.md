---
title: Ubuntu-Unable to locate package错误解决办法
index_img: /img_index/index/20211022-001.jpg
categories:
  - [服务器]
tags: [Ubuntu]
top: false
abbrlink: 53363
date: 2021-10-22 08:50:45
updated: 2021-10-22 08:50:45
desc: 解决Ubuntu系统上Unable to locate package的错误
keywords: Ubuntu, apt, apt-get, update
---

{% note success %}
在Windows Terminal上集成了Ubuntu Terminal，安装小火车sl的时候，出现以下的错误：
{% endnote %}
![](article_ubuntu_error.jpg)
{% label info@Ubuntu %}

<!--more-->
<hr />

Unable to locate package大概意思就是找不到需要安装的包，执行以下命令：
```
sudo apt-get update
```
这个命令会访问apt配置文件中所有的源，会读取源的软件列表保存在本地。以后使用apt、apt-get命令去install的时候会遍历本机保存的软件列表，所以需要定期对软件列表进行update命令更新源的软件列表。
在去执行刚才需要安装的命令。

{% raw %}
<div class="post_cus_note">拓展</div>
{% endraw %}
```
sudo apt-get upgrade
```
执行upgrade命令会把本机安装的packages与上述update更新的软件列表进行比对，如果版本低于软件列表，会进行更新操作。
