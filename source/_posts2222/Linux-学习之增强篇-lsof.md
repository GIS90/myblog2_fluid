---
title: Linux学习之增强篇-lsof
index_img: /img_index/index/20191206-001.jpg
categories:
  - [Linux]
tags: [Linux, Linux增强篇]
abbrlink: 35042
date: 2019-12-06 09:53:46
updated: 2019-12-09 09:53:46
desc: 记录Linux命令学习基础篇之lsof
keywords: linux, lsof, 服务器, 命令, 端口, 文件, 进程, shell, bash
---

#### 1、简介
{% note danger %}
Linux万能命令之lsof，<font size=6.5 color='red'>只有你想不到的，没有踏做不到</font>。
{% endnote %}

{% label default@Linux %} {% label info@lsof %} {% label danger@万物皆文件 %}

<!--more-->
<hr />

#### 2、推荐指数
```
🌟🌟🌟🌟🌟
```

#### 3、基本介绍

lsof = list opened files，大致的意思就是列举系统中被打开的文件。玩linux的人都知道：“linux万物皆文件”，目录、sockets文件、字符设备、块设备、管道符等等。所以在linux系统之内，不管干什么都会跟文件挂上边，lsof是打开这些被使用文件列表的一个工具。不过，在平常，我用的最多就是查端口以及文件被哪个进程使用，命令非常强大。

#### 4、文件类型

在关于lsof能进行查看的文件类型，做了一下分类统计，结果如下：

- 普通文件（.txt、.log等等）
- 目录
- 网络文件系统的文件
- 字符或设备文件
- (函数)共享库（lib文件目录下的，.so等）
- 管道、命名管道（.pip等）
- 符号链接（软链）
- 网络文件（socket相关文件，.sock）

应该还有其他的文件，本人刚入linux系统，不足的地方请各路神山补充，欢迎留言👏👏👏。

#### 5、安装

lsof命令是linux系统非自带的一个命令。

> 检查是否已安装

{% code %}
方式一：
lsof

方式二：
which lsof
{% endcode %}

出现***-bash: lsof: command not found ***，说明命令还没被安装，如果命令已存在，忽略安装步骤。

> 安装

{% code %}
[root@localhost ~]# yum install lsof
Loaded plugins: fastestmirror
base                                                                                | 3.6 kB  00:00:00
extras                                                                              | 3.4 kB  00:00:00
updates                                                                             | 3.4 kB  00:00:00
extras/7/x86_64/primary_db                                                          | 200 kB  00:00:00
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirrors.tuna.tsinghua.edu.cn
Resolving Dependencies
--> Running transaction check
---> Package lsof.x86_64 0:4.87-6.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================
 Package                Arch                     Version                      Repository              Size
===========================================================================================================
Installing:
 lsof                   x86_64                   4.87-6.el7                   base                   331 k

Transaction Summary
===========================================================================================================
Install  1 Package

Total download size: 331 k
Installed size: 927 k
Is this ok [y/d/N]: y
Downloading packages:
lsof-4.87-6.el7.x86_64.rpm                                                          | 331 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : lsof-4.87-6.el7.x86_64                                                                  1/1
  Verifying  : lsof-4.87-6.el7.x86_64                                                                  1/1

Installed:
  lsof.x86_64 0:4.87-6.el7

Complete!

{% endcode %}

整个安装过程问题不大，大大小小的lsof安装已经也装过10几次了，重没发生过意外，有问题的话可以邮件、微信联系我，一起来进行解决。

#### 6、输出内容

这个先简单介绍一下lsof输出信息的内容，具体格式如图：

![](lsof_fields.png)

> COMMAND

command，进程的名称

> PID

process id，进程标识符id

> TID

thread id，线程标识符id

> USER

user，进程所属用户

> FD

file description，文件描述符，应用程序可以通过文件描述符识别该文件，一般有以下取值：
- cwd：current work dirctory，应用程序启动的目录
- txt：program text (code and data)，该类型的文件是程序代码，如应用程序二进制文件本身或共享库
- lnn：library references (AIX)
- er：FD information error (see NAME column)
- jld：jail directory (FreeBSD)
- ltx：shared library text (code and data)
- mxx：hex memory-mapped type number xx
- m86：DOS Merge mapped file
- mem：memory-mapped file
- mmap：memory-mapped device
- pd：parent directory
- rtd：root directory
- tr：kernel trace file (OpenBSD)
- v86：VP/ix mapped file
- 0：表示标准输出
- 1：表示标准输入
- 2：表示标准错误

> TYPE

type，文件类型，常见的文件类型有以下几种：
- DIR：表示目录
- CHR：表示字符类型
- BLK：块设备类型
- UNIX：UNIX域套接字
- FIFO：先进先出(FIFO)队列
- IPv4：网际协议(IP)套接字

> DEVICE

device，指定磁盘的名称

> SIZE/OFF

size，文件的大小

> NODE

node，索引节点，在这里啰嗦几句，磁盘存储机制，来个比喻吧：每个磁盘是一个大房子，大房子里面有多个小房子用来存储东西，但是呢每个小房子都有自己的房间号1、2、3、4、5、6......就这样一直排，我们把需要存储的数据放在小房子里面，一个不够就两个、两个不够就三个、以此类推，但是有个记录表需要记录刚存储的数据所对应的房间号，这样物流存储与房间号就对应起来了，所谓的房间号就是现在看的NODE节点，NODE节点就是文件在磁盘上的标识。

> NAME

name，打开文件的确切名称

#### 7、参数详解

lsof参数有太多太多，这里不一一列举，只讲解一下常用的参数，如果有想深入了解的，请<font size=5.5 color='red'>man lsof</font>。

> -a

列出打开文件存在的进程

> -c <进程名>

列出指定名称进程所使用到的文件

> -d <文件>

列出打开指定文件描述的进程

> +d <目录>

列出目录下被打开的文件

> +D <目录>

递归列出目录下被打开的文件

> -n <目录>

列出使用NFS的文件

> -u <用户名>

列出指定用户打开的文件

> -p <进程id>

列出指定进程号所打开的文件

> -i <条件>

列出打开的套接字，过滤条件：tcp、udp、4、6、协议、:端口、 @ip

#### 8、常用命令

> 查看端口

```
lsof -i :80
```

![](lsof_port.png)

列出80端口目前打开的文件列表。

> 查看连接

```
# 所有网络连接信息
lsof -i

# TCP网络连接信息
lsof -i tcp

# UDP网络连接信息
lsof -i udp

# TCP连接方式、端口为8080的连接信息
lsof -i tcp:8080

# UDP连接方式、端口为8080的连接信息
lsof -i udp:8080
```

![](lsof_i.png)

> 指定进程名称

```
lsof -c nginx
```

![](lsof_c.png)

列出以nginx开头的进程打开的文件列表。

> 指定进程id

```
lsof -p 3215
```

![](lsof_c_id.png)

列出指定进程打开的文件列表。

> 指定用户

```
lsof -u root
```

![](lsof_user.png)

列出指定用户打开的文件列表。

> 查看指定目录被打开的文件（目录非遍历）

```
lsof +d /usr/local/
```

![](lsof_dd.png)

列出目录下被进程打开的文件列表。

> 查看指定目录被打开的文件（目录遍历）

```
lsof +D /usr/local/
```

![](lsof_D.png)

遍历搜索的方式列出目录下被进程打开的文件列表。

#### 9、结束语

本篇先写到这里吧，以上是我常用的命令以及参数说明，各种参数可以一起使用，如果不能满足大家的需求请见谅，毕竟本人是linux小白一枚，<font size=6.5 color='blue'>在不断尝试中去学习、去进步、去成长、去积累吧</font>，敬自己。

#### 10、学习参考

lsof简书：https://www.jianshu.com/p/be0c534c6a41
每日一命令lsof：https://www.cnblogs.com/peida/archive/2013/02/26/2932972.html

感谢上面的2篇文章。
