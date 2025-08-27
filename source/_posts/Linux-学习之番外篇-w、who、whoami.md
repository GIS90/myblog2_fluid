---
title: Linux学习之番外篇-w、who、whoami
index_img: /img_index/index/20191224-002.jpg
categories:
  - [Linux]
tags: [Linux, Linux番外篇]
abbrlink: 52265
date: 2019-12-24 08:54:11
updated: 2019-12-24 08:54:11
desc: 总结一下Linux在w、who、whoami在用于显示用户信息的相关操作
keywords: Linux, w, who, whoami, 用户, id, last
---


{% label default@Linux %} {% label info@w who whoami%} {% label warning@用户 %} {% label success@番外教程系列 %}
### 1、简介
{% note danger %}
简单总结一下<font color="#dd0000" size="5">w、who、whoami</font>，用于显示当前登录Linux的用户信息，在本篇结束的时候又额外赠送了2个命令，关于学习<font size=6.5 color='red'>多多益善。。。。。。</font>。
{% endnote %}

<!--more-->
<hr />

在Linux系统中，多账户进行ssh登录是常有的事，但是有时候会去查看一下登录的用户都有谁以及相关的登录信息。

### 2、总结

|  命令  |                         说明                         |
|:------:|:----------------------------------------------------:|
|   w    |               显示当前登录的用户及信息               |
|  who   | 显示当前已经登录的用户名、终端名称、登录时间及登录IP |
| whoami |                 显示当前用户的用户名                 |
|   id   |               用于check系统是有此用户                |
|  last  |             显示近期用户或终端的登录情况             |

#### 2.1、w

显示当前登录的用户及信息，除了这些还会显示当前登录了几个用户、系统当前的load。

```BASH
[root@bash ~]# w
20:22:25 up 15:55,  3 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1                      Tue21   15:27m  0.09s  0.09s -bash
root     pts/0    192.168.1.116    19:23    1.00s  0.12s  0.01s w
root     tty2                      07:43   19:29   0.02s  0.02s -bash
```

#### 2.2、who

显示当前已经登录的用户名、终端名称、登录时间及登录IP。

```BASH
[root@bash ~]# who
root     tty1         2019-12-24 21:23
root     pts/0        2019-12-24 19:23 (192.168.1.116)
root     tty2         2019-12-24 07:43
```
#### 2.3、whoami

显示当前用户的用户名。
```BASH
[root@bash ~]# whoami
root
```

{% raw %}
<div class="post_cus_note">额外赠送</div>
{% endraw %}

#### 2.4、id

用于显示用户的ID，以及所属群组的ID，主要用于check系统是有此用户。
```BASH
[root@bash ~]# id root
uid=0(root) gid=0(wheel) groups=0(wheel)
```

#### 2.5、last

显示近期用户或终端的登录情况。
```BASH
[root@bash ~]# last -n 5
root     pts/0        :0.0             Wed Apr 25 10:12   still logged in
root     pts/1        :0.0             Wed Apr 25 10:06 - 10:10  (00:03)
root     pts/0        :0.0             Wed Apr 25 10:06 - 10:10  (00:03)
root     pts/0        :0.0             Wed Apr 25 10:02 - 10:06  (00:04)
root     pts/0        :0.0             Wed Apr 25 09:51 - 09:51  (00:00)
```
