---
title: Crontab指定用户运行任务
index_img: /img_index/index/20201219-001.png
categories:
  - 服务器
tags:
  - Crontab
  - Linux
desc: Linux系统关于用指定用户去执行crontab任务
keywords: 'Crontab, Linux, 定时任务, 服务器'
abbrlink: 44008
date: 2020-12-19 15:23:09
updated: 2020-12-19 15:23:09
---

{% note warning %}
每一个用户都有属于自己的crontab，crontab -l的时候显示都是自己的定时任务，不便于管理。
把所有任务集中管理，并指定每个任务的执行用户，记录一下。
{% endnote %}



<!--more-->
<hr />

{% label primary@Crontab %} {% label info@定时任务 %}

> 系统环境

Contos7.5

> 操作

root用户。
- 打开/etc/crontab文件中添加。

```
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

# baidu自动推送
1 23 * * * mingliang.gao bash /home/mingliang.gao/crontab/auto_push_baidu/auto_baidu_push.sh

# db backup
30 02 * * * mingliang.gao bash /home/mingliang.gao/crontab/db_backup_task.sh > /dev/null 2>&1
```
这里面需要指定用户名，统一管理。
