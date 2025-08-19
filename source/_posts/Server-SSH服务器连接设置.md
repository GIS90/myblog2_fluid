---
title: 服务器SSH长连接设置
index_img: /img_index/index/20201213-001.png
categories:
  - [服务器]
  - [工具集]
tags:
  - SSH
  - 协议类
desc: SSH云服务器保持持久连接，防止几分钟空闲就会断开连接
keywords: 'SSH, 长连接, 配置, config, 协议类, 服务器'
abbrlink: 13423
date: 2020-12-13 22:39:07
updated: 2020-12-13 22:39:07
---

{% note warning %}
SSH经常服务器之后，只要几分钟不去操作服务器，几分钟就会断开，经常重启一个新的终端（iTerm：command+t）重新进行ssh连接。
这种影响操作的事情不能容忍，上网寻求帮助，自己做个记录。
{% endnote %}

{% label info@SSH %} {% label success@长连接 %}

<!--more-->
<hr />

ssh连接到，远程服务器，切换到root用户。

> sshd_config

开放一下3个配置。
```
TCPKeepAlive yes
ClientAliveInterval 60
ClientAliveCountMax 10
```
- TCPKeepAlive：保持TCP长时间连接。
- ClientAliveInterval：每隔xxx秒发送向客户端发送一次包，检测是否活动状态的间隔时间。
- ClientAliveCountMax：发包请求次数，达到指定次数未收到回应，主动断开连接。

设置完上面之后，如果客户端无响应，最多存留10分钟。

> 重启sshd服务

Root用户操作。
```
systemctl sshd restart
```

{% raw %}
<div class="post_cus_note">弊端</div>
{% endraw %}

w查看当前服务器登录用户。
- ssh连接中会存在重复用户。
- ssh连接长时间不断开也不操作，影响服务器性能，浪费资源。

{% raw %}
<div class="post_cus_note">建议</div>
{% endraw %}

- kill掉重复用户。
- 养成好习惯，不用的时候exit进行推出。

> kill终端

- w
列出当前登录用户信息，包含终端信息，第二列。
```
[mingliang.gao@VM-0-15-centos ~]$ w -s
 23:18:23 up 10 days, 11:17,  3 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM              IDLE WHAT
minglian pts/0    123.179.117.77    1:07m sshd: mingliang.gao [priv]
minglian pts/1    123.179.117.77    1:04m -bash
minglian pts/3    123.179.117.77    7.00s w -s
```
- ps -t pts/1
查询终端进行进程ID。
- kill 进程ID

或者直接一个命令：
```
pkill -kill -t pts/1
```

<font size=6.5 color='red'>内容希望有帮助。。。。。。</font>
