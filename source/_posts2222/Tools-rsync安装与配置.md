---
title: Rsync安装与配置
index_img: /img_index/index/20210408-001.jpg
categories:
  - 工具集
tags:
  - Rsync
abbrlink: 11384
date: 2021-04-08 21:24:12
updated: 2021-04-08 21:24:12
desc: 关于rsync的基础环境搭建与命令实践
keywords: Rsync, rsync, 命令, 参数, 安装, 搭建, 环境, crontab, yum
---

{% note primary %}
项目上有需要对服务器之间的数据进行同步备份需求，想到了之前在Qunar使用过的开源同步软件RSYNC，这里做个简单的基础使用说明，主要是安装与配置。
{% endnote %}



{% label primary@RSYNC %} {% label default@数据同步 %} {% label danger@服务器 %}

<!--more-->
<hr />

RSYNC一款开源的数据备份软件，主要有一下几个特点：
- 跨平台，支持Windows与Linux双向同步
- 采用1服务端-N客户端运行模式，实现多台服务器同时请求
- 与其他命令结合使用方便，例如crontab、sersync（实时同步）
- 操作简单，安装完成后修改配置即可运行
- 实现增量/全量备份

之前使用是基于Linux系统的sersync+sync搭建的实时同步环境，当前项目上没不需要实时同步数据的要求，所以只搭建了rsync的使用，由于项目上有其他比较着急的事情，后续准备再升级到实时同步环境。

![](article_rsync.jpeg)

### 1、环境简介

| ID  |  NAME   |         SYSTEM         | DESC                                |
|:---:|:-------:|:----------------------:|:----------------------------------- |
|  1  | server1 |       RedHAT6.4        | 数据源服务器，用于做备份的SERVER    |
|  2  | server2 | Windows Server 2008 R2 | 主要做备份的服务器，安装rsync客户端 |

项目上数据源的服务器是Linux的，用来说备份的Server端，这个是不可以更改的系统类型，其他服务器是Windows的，用来做备份的Client。

### 2、下载地址

- Windows：https://www.itefix.net/cwrsync
- Linux：https://rsync.samba.org/

Windows需要下载cwrsync软件包，官网下载即可。
Linux安装采用在线安装，也可以下载源码包进行离线安装，均可。

### 3、安装

关于安装对于系统的不同分别进行说明，其中Linux的安装讲3种方式安装，一些Linux的基础命令使用还是需要掌握的，不会的童鞋需要恶补一下了，废话不说，咱们开始整。

> Linux

#### 3.1、在线安装

前提保证服务器连网，可以充分使用yum。
```
# 查询yum源是否有rsync软件
yum search rsync
```
使用search进行查找，如果没有，需要进行yum源的设置；如果有，进行install安装即可。
```
yum -y install rsync
```

#### 3.2、离线安装

```
wget https://download.samba.org/pub/rsync/src/rsync-3.2.3.tar.gz
tar -zxvf rsync-3.2.3.tar.gz
./configure --prefix=/usr/local/rsync
make
make install
```
使用wget或者官网下载rsync源码包，并移动到服务器上进行解压与安装，具体命令如上。

#### 3.3、镜像安装

```
mount -o loop /root/mingliang.gao/rhel-server-6.4-x86_64-dvd.iso /mnt/
cd /mnt/Packages
ls -l | grep rsync
rpm -ivh rsync-3.0.6-9.el6.x86_64.rpm
umount /mnt/
```
镜像安装有个弊端，就是需要下载与服务器系统版本一致的ios镜像包，移动到服务器进行挂载、安装。

{% raw %}
<div class="post_cus_note">安装建议</div>
{% endraw %}
关于Linux的安装，建议使用在线安装>镜像安装>离线安装，如果是内网不能连网的情况考虑镜像安装与离线安装，离线安装rpm方式需要一定的依赖，如果是一台新的服务器而且还不能连网，安装起来很费劲，不建议使用源码安装。

> Windows

Windows安装客户端比较简单，直接官网进行下载：https://www.itefix.net/cwrsync
![](cwrsync.png)

下载好的包进行解压，把对应的路径加入系统PATH即可。


### 4、配置

配置主要是基于Server端进行配置，需要开启服务，Client进行数据获取，主要配置的操作如下：

- mkdir -p /etc/rsyncd
- cd /etc/rsyncd
- rsyncd.passwd配置文件
    touch rsyncd.conf
    粘贴下面的内容到rsyncd.conf配置文件

    ```
    # /etc/rsyncd: configuration file for rsync daemon mode
    # See rsyncd.conf man page for more options.
    port = 873
    uid = root
    gid = root
    use chroot = yes
    read only = no
    write only = no
    # IP白名单
    hosts allow = *
    hosts deny = *
    max connections = 4
    # motd file = /etc/rsyncd/rsyncd.motd
    pid file = /var/run/rsyncd.pid
    lock file = /var/run/rsyncd.lock
    transfer logging = yes
    log format = %t %a %m %f %b
    log file = /var/log/rsync.log
    exclude = lost+found/
    timeout = 900
    ignore nonreadable = yes
    dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2
    list = no
    ignore errors
    auth users = root
    secrets file = /etc/rsyncd/rsyncd.passwd
    
    # 数据路径配置
    [gtp]
    path = /home/gtp/data
    ```
    主要更改[gtp]数据模块配置与hosts allow
- rsyncd.passwd
    touch rsyncd.passwd
    内容：root:Root123（服务器连接的用户、用户密码，用英文:分割）
- chmod 600 rsyncd.passwd

    <font color='red'>重点，需要设置文件的600模式，其他模式不允许。</font>

### 5、配置文件说明

> 全局参数

- port：服务端口，默认873
- uid：用什么用户运行
- gid：运行用户所属组
- use chroot：若为 true，则 rsync 在传输文件之前首先chroot到 path 参数所指定的目录下。这样做的原因是实现额外的安全防护，但是缺点是需要 root 权限，并且不能备份指向 path 外部的符号连接所指向的目录文件。
- motd file：同步开始打印的内容
- pid file：rsync进程的.pid文件，文件里面存放PID
- lock file：rsync进程的.lock文件

> 模块参数

- hosts allow：IP白名单，*或者注释代表全部允许
- hosts deny：IP黑名单，不允许同步的IP
- max connections：最大连接数量，0为无限制
- transfer logging：使rsync配置的记录日志文件
- log format：日志格式
- log file：日志
- exclude：排除哪些目录不同步
- timeout：一次连接的最大时长
- ignore nonreadable：
- dont compress：用来指定在传输之前不进行压缩处理的文件格式
- list：指定当客户请求列出可以使用的模块列表时，该模块是否应该被列出。如果设置该选项为 false，可以创建隐藏的模块
- ignore errors：可以忽略一些无关的IO错误
- auth users：授权的用户，需要与发起方保持一致，多个用户用,分隔
- secrets file：密钥文件，里面是服务器连接的用户、用户密码

> 数据模块格式

可以定义多个，格式如下：
```
[模块名称]
path = 具体路径
comment = 同步数据模型的描述
```
官网rsync配置文件参数说明：https://download.samba.org/pub/rsync/rsyncd.conf.5

### 6、运行

> Linux服务端


- 检查端口
```
netstat -nltp|grep 873 或者 lsof -i:873
```
    服务端默认的端口是873，先check下服务器的873是否被占用，如果没有直接启动服务即可；被占用需要更改上面的配置文件中的port进行端口更换再进行启动。
- 运行
```
rsync --daemon --config=/etc/rsyncd/rsyncd.conf
```
    启动服务。
- 查看
```
ps -ef | grep rsync
```

> Windows客户端

```
rsync --port 873 -avzP --password-file=/cygdrive/D/cwRsync/rsyncd.passwd root@16.19.209.68::newgtp /cygdrive/E/newdata
```
如果出现没有发现rsync命令，就是没有把rsync的安装路径加入系统PATH。不想加入系统PATH就把rsync的路径补全，采用路径+rsync的方式运行。

参数说明：
```
-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
-v, --verbose 详细模式输出
--progress 在传输时现实传输过程
-z, --compress 对备份的文件在传输时进行压缩处理
--delete 删除那些DST中SRC没有的文件
--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件
--password-file=FILE 从FILE中得到密码
--port=PORT 指定其他的rsync服务端口
--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件
--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
```

### 7、其他

> umount问题：umount: /mnt: device is busy.

解决方案：
fuser -m /mnt/ 查看使用文件的进程id
kill或者exit、重新ssh
重新umount

> 重新启动

```
ps -ef | grep rsync
kill 进程ID
rsync --daemon --config=/etc/rsyncd/rsyncd.conf
```

### 8、学习参考

yum命令与源设置：https://zhuanlan.zhihu.com/p/71906253
Linux编译详解：https://www.cnblogs.com/tinywan/p/7230039.html
rsync安装：https://www.cnblogs.com/champaign/p/9082137.html
rsync参数：https://www.cnblogs.com/koushuige/p/9162920.html
rsync配置文件参数说明：https://download.samba.org/pub/rsync/rsyncd.conf.5
