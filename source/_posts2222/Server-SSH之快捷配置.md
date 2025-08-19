---
title: SSH之快捷配置
index_img: /img_index/index/20190829-001.png
categories:
    - [服务器]
    - [工具集]
tags: [SSH, 协议类]
abbrlink: 24280
date: 2019-08-29 19:04:09
updated: 2019-08-29 19:04:09
desc: SSH基础学习之快捷配置，关于config配置文件的介绍
keywords: SSH, 快捷, 配置, config, 协议类, 服务器, 22
---

### 背景

{% note warning %}
上篇文章介绍了有关<font color='red' size=4.5>SSH协议</font>的基础知识，正常工作中都是通过下列命令进行ssh连接：

{% code %}
ssh 用户名@服务器IP
{% endcode %}


<!--more-->
<hr />

工作中，需要ssh连接到服务器，然后每次需要输入服务的IP或者机器名，而且连接到一台服务器后，还可能跳转到另外一台服务器，每次还需要输入密码，个人感觉比较麻烦。通过配置ssh_config的方式简化连接命令，在ssh远程连接上更加高效。
{% endnote %}

{% label default@SSH %} {% label primary@服务器连接 %} {% label info@SSH配置 %}


### 结果对比

> 配置前

{% code %}
ssh 用户名@服务器IP
{% endcode %}
每次都需要输入密码

> 配置后

```
ssh 别名
```
每日只需输入一次密码

### 配置详解

1 进入到***~/.ssh***目录，查看是否有***config***文件，如果没有这个文件，需要通过***touch config***手动进行建立。

![](ssh_config.png)

2 配置文件内容
```
Host XXXX
HostName IP
User mingliang.gao
ControlMaster auto
ControlPath ~/.ssh/master-%r@%h:@%p
ControlPersist yes
PasswordAuthentication no
IdentityFile ~/.ssh/id_rsa
```
- Host：ssh进行连接的别名
- HostName：服务器的IP或者机器名
- User：用户名
- Port：端口
- ControlMaster：是否多个会话连接用一个session文件的模式，需要配合ControlPath、ControlPersist一起使用
- ControlPath：保存的session会话文件（%r远程连接的用户，%h远程连接的ip，%s远程连接的端口）
- ControlPersist：连接是否保持长连接
- PasswordAuthentication：是否需要密码认证
- IdentityFile：私钥文件

如果需要了解更多的参数，请<font color='red' size=5>***man ssh_config***</font>进行查看。

### 结果

配置完之后，每次只需要***ssh 别名***就可以连接到指定配置的服务器，而且在会话session结束之前只需要第一次密码即可。
