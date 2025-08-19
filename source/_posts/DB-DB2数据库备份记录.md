---
title: DB2数据库备份记录查询
index_img: /img_index/index/20220617-001.jpg
categories:
  - [数据库]
tags: [DB2]
abbrlink: 64563
date: 2022-06-17 21:05:52
updated: 2022-06-17 21:05:52
desc: DB2数据库备份记录查询
keywords: DB2, 数据库, 备份
---



{% note primary %}
有一天手动备份数据库，想看下数据库备份时间，于是找到了***db2 list history backup all for 数据库***命令。
{% endnote %}

{% label primary@DB2 %} {% label danger@backup %}

<!--more-->
<hr />

直接DB2CMD执行：
```
db2 connect to 数据库 user pas using pas
db2 list history backup all for 数据库
```

结果：
```
Op Obj Timestamp+Sequence Type Dev Earliest Log Current Log  Backup ID
 -- --- ------------------ ---- --- ------------ ------------ --------------
  B  D  20220802175839001   F    D  S0000000.LOG S0000000.LOG
 ----------------------------------------------------------------------------
  Contains 9 tablespace(s):

 00001 SYSCATSPACE
 00002 TBS_MXZ
 00003 SYSTOOLSPACE
 00004 TBS_PAS
 00005 TBS_JKSJ
 00006 TBS_JXDX
 00007 TBS_NBZZ
 00008 TBS_IDX
 00009 TBS_LSB
 ----------------------------------------------------------------------------
    Comment: DB2 BACKUP YKSPAS3 OFFLINE
 Start Time: 20220802175839
   End Time: 20220802231447
     Status: A
 ----------------------------------------------------------------------------
  EID: 48531 Location: /home/db2inst1/SNPAS/PAS_BACKUP/PAS_BACKUP20220731
```
- Start Time: 开始时间
- End Time: 结束时间

{% raw %}
<div class="post_cus_note">拓展</div>
{% endraw %}

查看数据库备份是否可用：
```
db2ckbkp -h /home/db2inst1/SNPAS/PAS_BACKUP/PAS_BACKUP20220731
```
-h 后面是数据库备份文件，如果是可用的备份文件最后会有successful。
