---
title: SSH之免密码登录
index_img: /img_index/index/20190903-001.png
categories:
  - [服务器]
  - [工具集]
tags:
  - SSH
  - 协议类
desc: SSH基础学习之免密码登录服务器
keywords: 'SSH, 快捷, 配置, config, 协议类, 服务器, 免密码'
abbrlink: 1431
date: 2019-09-03 21:34:27
updated: 2019-09-03 21:34:27
---

#### 背景

{% note warning %}
前几天的文章中介绍了SSH的快捷配置，今天讲述一下SSH免密码登录服务器，这个操作在玩服务器中经常用到，建议收藏。
{% endnote %}




<!--more-->
<hr />

{% label default@SSH %} {% label success@免密码 %} {% label info@SSH配置 %}

#### 正文

> 生成密钥公钥

```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
```
这个命令会产生一个公钥文件id_rsa.pub和密钥文件id_rsa，
* -t rsa：表示使用密钥的加密类型
* -P ''：表示不需要密码登录
* -f ~/.ssh/id_dsa：表示密钥存放的路径为用户/.ssh/id_dsa

> 公钥上传服务器

```
ssh-copy-id 用户@服务器IP
输入用户登录密码
```
执行这个命令，会把刚生成公钥文件内容复制到服务器~/.ssh/authorized_keys文件中。
也可以用其他方式，只要把公钥复制到服务器authorized_keys文件中即可。

这样就实现SSH免密码登录服务器，加上上篇SSH配置中config，以后直接ssh XXXX即可登录服务器。

> SSH长连接配置

第一步 编辑服务器的 sshd_config
```
vim  /etc/ssh/sshd_config
```
第二步 找到如下配置并且修改参数
```
ClientAliveInterval 60      ## 这个属性表示超时时间(单位是秒)，修改为 60,需要可以加到600
ClientAliveCountMax 10     ## 这个属性是超时链接次数，允许你超时的次数 10
```
这个配置表示超时时间为一分钟，超时连接次数为10次，也就是允许长连接十分钟不操作
第三步 保存配置并生效
```
systemctl restart sshd
```


#### 特别说明

服务器ssh目录以及authorized_keys文件的权限：

- chmod 700 ~/.ssh/
- chmod 600 authorized_keys
