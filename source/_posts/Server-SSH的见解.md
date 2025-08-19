---
title: SSH协议的基础知识
index_img: /img_index/index/20190819-001.png
categories:
  - [服务器]
  - [工具集]
tags: [SSH, 协议类]
abbrlink: 62041
date: 2019-08-19 10:07:42
updated: 2019-08-19 10:07:42
desc: SSH协议的个人见解，关于基础知识的介绍
keywords: SSH, 快捷, 配置, config, 协议类, 服务器, 22
---

### 1、背景
{% note warning %}

因为一些原因，把github.io重新在github搭建一下，但是**hexo deploy**去提交代码的时候，发生代码提交失败的情况。原来在GIS90.github.io Repository上没有进行***Deploy keys***的配置，导致push失败。

配置好***Deploy keys***的配置，代码成功提交，但是为何配置完key就可以成功提交呢？原理是<font color='red' size=4.5>SSH协议</font>。

{% endnote %}


<!--more-->
<hr />
{% label default@SSH配置 %} {% label primary@SSH安装 %} {% label success@协议 %} {% label info@服务器 %}

### 2、定义

***The SSH protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network. For details of how the SSH protocol works, see the protocol page. To understand the SSH File Transfer Protocol, see the SFTP page.***

***The SSH protocol (also referred to as Secure Shell) is a method for secure remote login from one computer to another. It provides several alternative options for strong authentication, and it protects the communications security and integrity with strong encryption. It is a secure alternative to the non-protected login protocols (such as telnet, rlogin) and insecure file transfer methods (such as FTP).***

直接摘录了官网的原话，不多说了直接捞干的，大概意思就是说<font color='red' size=4.5>SSH协议是一种连接服务器的一种简单、安全、可靠的连接方法。</font>

### 3、验证方式

目前，后端开发、运维等工作中经常需要登录远程服务并操作，常用的连接方式都是ssh，方式如下：
```
ssh 用户名@IP
ssh 用户名@机器名
```
使用ssh登录的时候，验证方式主要有2种：
> 密码验证

使用用户名和密码进行登录，两者匹配才可以登录，但是密码认证有以下的缺点：

- 密码泄漏。一个帐户多个人进行使用，需要让所有使用人都知道密码，很容易密码泄露。
- 另外，多账户使用修改密码时必须通知所有人，否则就会造成其他人登录失败。

> 公钥验证

使用公钥、私钥的方式就行验证。保证了多个用户可以通过各自的密钥登录到服务器，互不干扰，而且认证也可以允许使用空密码，省去每次登录都需要输入密码的麻烦。

### 4、配置

通过<font color='red' size=4.5>ssh-keygen</font>命令进行生成公钥私钥，下面详细说明。

1、执行***ssh-keygen -t rsa***命令。
通过***ssh-keygen --help***可知，-t是加密的参数，最常用。除了rsa，还有dsa、ecdsa、ed25519，其中rsa是安全的加密方式。

2、输入要生成的文件名，默认回车即可。

3、输入密码，默认回车即可。

4、输入确认密码，，默认回车即可。

5、出现下列图案代表成功。
```
+--[ RSA 2048]----+
|    .+=*..       |
|  .  += +        |
|   o oo+         |
|  E . . o        |
|      ..S.       |
|      .o .       |
|       .o +      |
|       ...oo     |
|         +.      |
+-----------------+
```

具体的ssh-keygen参数可以通过***ssh-keygen --help***进行查看，这里不作详解。

### 5、原理讲解

1. #### 定义

    这里主要针对于公钥私钥的验证方式进行讲解，通过执行ssh-kengen命令可以得到**公钥、私钥**，都是经过加密后的文件。
    公钥：能被其他人知道的加密文件，id_rsa.pub。
    私钥：只能自己知道的加密文件，id_rsa。

    只要私钥不被泄漏，就不会出现任何指定用户登录的问题。

    ![](ssh_keys.png)

2. #### 流程图

    ![](ssh_yuanli.png)

3. #### 验证流程

    1、Client通过***ssh-keygen***命令生成公钥和私钥。

    2、Client将自己的公钥***id_rsa.pub***存放到Server服务器上的认证文件。

    3、Client通过***ssh***命令将用户、IP发送请求连接指定的服务器。

    4、Server收到到请求之后，先去authorized_keys中进行查找是否有指定的用户，如果有将进行下一步，无此用户拒绝验证连接。

    5、验证文件中包含指定用户，Server将生成一个随机字符串通过***公钥**进行加密，发送给Client。

    6、Client接收到Server加密的字符串，利用***私钥***进行解密，解密后在将这个随机字符串再发送给Server。

    7、Server接收到Client发送的解密字符串与生成的原始随机字符串进行比对，如果正确就让客户端登录，否则拒绝。

### 6、学习参考

SSH官网：https://www.ssh.com/ssh/
