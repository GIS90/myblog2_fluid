---
title: Linux学习之基础篇-ps
index_img: /img_index/index/20200221-001.jpg
categories:
  - [Linux]
tags: [Linux, Linux基础篇]
abbrlink: 9834
date: 2020-02-21 18:45:32
updated: 2020-02-21 18:45:32
desc: 记录Linux命令学习基础篇之ps
keywords: linux, cd, 服务器, 命令, shell, bash
---



{% label default@Linux %} {% label info@process status %} {% label success@基础教程系列 %}


ps，运维命令利器，查看运行的进程命令，全名：<font size=6.5 color='red'>process status</font>


<!--more-->
<hr />

### 1、简介

{% note warning %}
学习ps的用法【ps 参数】，显示系统进程。
{% endnote %}

### 2、介绍

Linux中有很多命令可以查询正在运行的进程的信息，其中**ps**与**top**最为常用，至于二者的区别，可以认为ps是当前进程的镜像，是静态的，top可以动态的显示进行信息、内存、CPU等信息。
本文讲解ps的用法，至于top的使用会在后续中写出。

### 3、正文

{% raw %}
<div class="post_cus_note">来个美图  放飞一下</div>
{% endraw %}

![](hushi.jpg)

{% raw %}
<div class="post_cus_note">开始正餐</div>
{% endraw %}

#### 3.1、格式

```
ps [参数]

ps [参数] | grep [内容]
```

#### 3.2、参数说明

由于ps的参数过多，这里常用的只列举常用参数，如有详细了解请<font color="red" size="5">man进行查询！！！</font>。

> -a

显示同一终端下的所有程序。

> -A

显示所有进程。

> c

显示进程的真实名称。

> -C<指令名称> 　

指定执行指令的名称，并列出该指令的进程的状况。

> -N

反向选择。

> -e

等于“-A”。

> e

显示环境变量。

> f

显示进程间的相互关系。

> -p<进程识别码> 　

指定进程识别码，并列出该进程的状况。

> -H

显示树状结构，表示进程间的相互关系。

> r

显示当前终端的进程。

> -t<终端机编号> 　

指定终端机编号，并列出属于该终端机的进程的状况。

> T

显示当前终端的所有程序

> u

指定用户的所有进程。

> -G<群组识别码>
-g<群组识别码>

列出属于该群组的进程的状况，也可使用群组名称来指定。

> --help

显示帮助信息。

> --version

显示版本显示。

#### 3.3、常用命令

> ps -ef

![](ef.jpg)

- UID：进程拥有者ID
- PID：进程的ID
- PPID：父级进程的ID
- C：进程CPU使用的百分比
- STIME：进程的启动时间
- TTY：登录终端
- TIME：进程使用掉CPU的时间
- CMD：执行的命令

> ps -aux

![](aux222.jpg)

- USER：用户名
- %CPU：进程占用的CPU百分比
- %MEM：占用内存的百分比
- VSZ：进程使用的虚拟內存量（KB）
- RSS：进程占用的固定內存量（KB）（驻留中页的数量）
- TTY：登录终端
- STAT：进程的状态

| 状态码 |     简述     | 详情                                                              |
|:------:|:------------:|:----------------------------------------------------------------- |
|   D    |   不可中断   | 收到信号不唤醒和不可运行, 进程必须等待直到有中断发生              |
|   R    |     运行     | 正在运行或在运行队列中等待                                        |
|   S    |     中断     | 中断 sleeping                                                     |
|   T    | 停止或被追踪 | 进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行         |
|   Z    |   僵尸进程   | 进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放 |
|   W    |   内存交换   | 进入内存交换                                                      |

- START：进程被触发启动时间
- TIME：进程实际使用CPU运行的时间
- COMMAND：执行的命令

如果需要显示进程的真是名称：
```
ps -auxc
```

> ps -axjf

![](axjf.jpg)

信息内容与**ps -aux**差不多，唯一的区别就是在进程COMMAND显示这这块，***ps -axjf***树形显示进程，比较清晰。

> ps -ef -u root

查询指定用户的进程。

![](ef.jpg)

> 内容输出

- 文本
```
ps -ef > /home/mingliang.gao/cur_pro.txt
```

- 翻页
```
ps -ef | less
```

### 4、补充

> grep

用ps去查询想要的进程，我一般都是配合上***grep***。
```
# 格式
ps -ef | grep "XXXXX"
```

> pstree

如果想以树节点形式显示整体的进程，用这个命令，一般系统没有，需要进行安装。
- 在 Mac OS上
```
brew install pstree
```

- 在 Fedora/Red Hat/CentOS
```
yum -y install psmisc
```

- 在 Ubuntu/Debian
```
apt-get install psmisc
```

### 5、学习

菜鸟说明：https://www.runoob.com/linux/linux-comm-ps.html
man手册：http://man7.org/linux/man-pages/man1/ps.1.html

推荐使用man手册，是最全面的文档，在文档的***EXAMPLES***部分，会有常用的命令。

![](examples.jpg)


<font color="red" size="5">希望对大家有所帮助！！！</font>
