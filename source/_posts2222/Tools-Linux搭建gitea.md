---
title: Gitea代码库的环境搭建
index_img: /img_index/index/20220222-001.png
categories:
  - 服务器
tags:
  [Linux, git, gitea]
desc: 服务器搭建gitea代码库管理项目文件
keywords: 'Linux, git, gitea'
abbrlink: 3042
date: 2022-02-22 21:43:18
updated: 2022-02-22 21:43:18
---



### 1、简介
{% note primary %}

最近基于项目管理文件的事情进行了一些探索，发现github不止可以用来更新、存储代码，也可以作为一个文件仓库存放项目的各类文件，对Github、Gitlab、Gogs、Gitea、Gitee进行了一些学习，也查下了网上对Gitea的呼吁声蛮大的，决定对测试服务器上进行部署使用，看看使用的效果咋样。
<font color='red' size=4.5>学习对Gitea的安装与使用</font>！！！。
{% endnote %}

{% label danger@Git %} {% label info@Gitea %} {% label primary@文件仓库 %}

<!--more-->
<hr />

面对项目上建立文件管理仓库，首先想到的就是gitub，平常用这个管理代码项目也居多，但是对于安装集中式管理还是分布式管理当时也纠结了一下，对于统一管理SVN也还是不错的，但是个人更倾向与GIT，下面对于遇到的概念或者安装、部署都做一个记录说明。

### 2、版本管理

#### 集中式版本控制

集中管理的服务器保存所有文件的修订版本，同步更新的时候，需要要先从中央服务器取得最新的版本，然后继续各种处理，最后，再把更新的内容推送给中央服务器。
典型的代表SVN，下面是结构图：
![](svn.png)

#### 分布式版本控制

分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
![](git.png)


### 3、仓库的选择

之前的单位用的就是Gitlab用来管理内部代码，功能也十分优秀，但是对于几个人维护的项目有点过于强大，所以选择了Gitea小型，够用而且速度没得说。
#### Github

全球最大的代码托管平台。
#### Gitlab

是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的Web服务。
#### Gogs

目标是打造一个最简单、最快速和最轻松的方式搭建自助 Git 服务。使用 Go 语言开发，并且支持所有平台，包括 Linux、Mac OS X、Windows 以及 ARM 平台。
#### Gitea

Gogs的一个分支（28.8K）。
#### Gitee

是开源中国（OSChina）推出的基于Git的代码托管服务，包括三个版本，分别是：社区版、企业版和高校版。

### 4、资源下载

对于安装部署Gitea环境，需要提前准备好数据库、GitForWindows、Gitea这3个软件就可以，在项目上我选择了MariaDB数据库。
- Gitea：https://dl.gitea.io/gitea
- Mariadb数据库：https://mariadb.org/download/?t=mariadb&o=true&p=mariadb&r=5.5.68
- Git客户端：https://gitforwindows.org/

为了方便大家统一进行下载，也准备了网盘下载地址：
链接: https://pan.baidu.com/s/1AUolZ1FrqLXvnVU1vRr2OA
提取码: pa3s

### 5、安装

> 数据库

安装略。
初始化语句：
```
create database gitea default character set utf8 collate utf8_general_ci;
create user 'gitea'@'%' identified by '123456';
grant all privileges on gitea.* to 'gitea'@'%';
flush  privileges;
```

> GitForWindows

略。

以上数据库与GitForWindows请自行安装部署，也很简单。

> Gitea

Windows安装部署：
1.文件放在D:\gitea
2.启动：双击下载文件

可选操作：加入系统服务。
添加服务：sc create gitea start= auto binPath= "\"D:\gitea\gitea.exe\" web --config \”D:\gitea\custom\conf\app.ini\""
删除服务：sc delete gitea


Linux安装部署：
1.下载
2.文件放在/opt/www/gitea/gitea
3.修改权限
chmod 777 gitea-1.x.x-linux-amd64
4.启动 && 后台启动
nohup /opt/www/gitea/gitea web > /dev/null 2>&1 &

5.其它方式：
https://docs.gitea.io/zh-cn/linux-service/


### 6、Gitea命令参数

以下是Gitea命令参数的详情，常用的就是web、dump这2个命令，详情请参考官网：https://docs.gitea.io/zh-cn/command-line/
```
COMMANDS:
   web              Start Gitea web server 启动服务
   serv             This command should only be called by SSH shell
   hook             Delegate commands to corresponding Git hooks
   dump             Dump Gitea files and database 备份
   cert             Generate self-signed certificate
   admin            Command line interface to perform common administrative operations
   generate         Command line interface for running generators
   migrate          Migrate the database
   keys             This command queries the Gitea database to get the authorized command for a given ssh key fingerprint
   convert          Convert the database
   doctor           Diagnose problems
   manager          Manage the running gitea process
   embedded         Extract embedded resources
   migrate-storage  Migrate the storage
   docs             Output CLI documentation
   dump-repo        Dump the repository from git/github/gitea/gitlab dump其他仓库
   restore-repo     Restore the repository from disk
   help, h          Shows a list of commands or help for one command
```

### 7、访问
访问本机地址：127.0.0.1:3000
默认端口：3000

初始化gitea，进行下一步系统配置。

### 8、配置

> 数据库配置

![](pz1.png)
> 基本配置

![](pz2.png)
> 可选配置

![](pz3.png)
> 用户配置

![](pz4.png)
> 管理配置

![](pz5.png)

### 9、目录说明

Gitea系统初始化成功会让自动建立一个管理员，对于上述配置后对应的目录也会自动生成。

> custom

配置文件目录，包含项目全局、数据库、仓库存储位置、访问服务、服务等各项配置。
> data

数据存放目录。
> log

日志存储目录。
> gitea执行文件

用于启动服务。

### 10、Gitea应用

具体Gitea的使用也没什么难度，主要就是创建仓库、维护等操作，对于管理员有个管理权限，部署成功后接下来就是对Gitea的探索。
创建仓库：
- 拥有者
- 仓库名称
- 描述
- 模板
- 工单标签
- .gitignore
- 授权许可
- 默认分支
- 签名信任模型

![](create.png)

### 11、Git使用

本来不打算介绍Git的使用了，但是把基础的操作功能记录下，对于初学者也可能比较友好，在Github上搞几个项目Git命令自然而然就会了，基础的功能还是比较好记的，有兴趣的人可以查查官网，命令超级多，而且参数也不少。

> GIT初始化仓库

方式一：
```
gitea创建仓库
git clone 仓库URL
```

方式二：
```
git init 仓库名称
git remote add origin 仓库URL
```
> GIT提交暂缓区

全量提交：
```
git add .
```
指定文件：
```
git add 指定文件
```
> GIT提交本地仓库

```
git commit -m “提交内容”
```
> GIT提交远程仓库与更新

提交远程仓库：
```
git push -u origin master
```
更新远程仓库：
```
git pull origin master
```
这里主要介绍Gitea安装与使用，对于git命令的学习请参考下列学习地址。

### 12、学习参考

- GIT官网：https://git-scm.com/book/zh/v2
- GIT基础学习：https://www.runoob.com/manual/git-guide/
- GIT菜鸟：https://www.runoob.com/git/git-basic-operations.html





<font color='red' size=4.5>每天坚持学习！</font>
