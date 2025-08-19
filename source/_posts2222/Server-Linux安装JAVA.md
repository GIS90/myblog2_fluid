---
title: Redhat7.6安装JAVA1.7与配置
index_img: /img_index/index/20220705-001.jpg
categories:
  - [服务器]
tags: [Linux, Java, 部署教程系列]
abbrlink: 13502
date: 2022-07-05 22:15:24
updated: 2022-07-05 22:15:24
desc: Redhat7安装JAVA
keywords: Linux, Java, Redhat7
---

{% label primary@Java %} {% label info@Redhat7 %}

<!--more-->

<hr />


#### 1、版本说明

|    名称    |                    版本                     |
|:----------:|:-------------------------------------------:|
| 服务器系统 | Red Hat Enterprise Linux Server release 7.6 |
|    Java    |                     1.7                     |


Linux安装方式本教程介绍2种。

#### 2、YUM安装


方式一：Yum安装

1.查询java版本
```
yum search java
```
![](461662564886_.pic.jpg)

2.安装指定版本：
```
yum -y install java-1.7.0-openjdk.x86_64
```
- 参数-y：yes，一路安装，不用交互
- 参数install：安装

配置在后面统一说明。



#### 3、源码安装

方式二：源码安装


1、打开Java下载地址，找到Java7SE（标准版），进行下载：
https://www.oracle.com/java/technologies/javase/javase7-archive-downloads.html
Linux版本，下载的时候根据安装系统选择对应的版本、位数：
![](471662565026_.pic.jpg)

2、下载好之后，使用连接工具的文件传输功能把源码Java传输到服务器。
![](481662565060_.pic.jpg)

3、创建Java安装目录
```
mkdir /usr/local/java
```
![](491662565089_.pic.jpg)

4、解压Java源码到建立的Java安装目录
```
tar -zxvf jdk-7u80-linux-x64.tar.gz -C /usr/local/java
```
参数说明 ：
- -zxvf 不解释了。
- -C不常用，解释一下：代表切换目录，change dir


#### 4、环境变量配置

以上的安装方式都需要配置环境变量，操作如下：
1、打开/etc/profile文件，加入以下配置：
```
vim /etc/profile
# -------------------------------------------------------------------
# java set environment
JAVA_HOME=/usr/local/java/jdk1.7.0_80
JRE_HOME=$JAVA_HOME/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME
export JRE_HOME
export CLASS_PATH
export PATH
#--------------------------------------------------------------------
:wq 保存
```
- JAVA_HOME：Java安装路径
- JRE_HOME：Jre安装路径
- CLASS_PATH：Java class path

2、变量生效
```
source /etc/profile
```

3、添加软连接，是命令在bin下
```
ln -s /usr/local/java/jdk1.7.0_80/bin/java /usr/local/bin/java
```

4、测试Java命令
```
java -version
```
![](501662565254_.pic.jpg)

{% raw %}
<div class="post_cus_note"> =========== END =========== </div>
{% endraw %}

#### 5、部署教程系列

- LINUX+ORACLE部署安装：http://pygo2.top/articles/40987/
- Linux安装JAVA1.7与配置：http://pygo2.top/articles/13502/
- Windows安装JAVA1.7与配置：http://pygo2.top/articles/1151/
- Windows安装Tomcat7与配置：http://pygo2.top/articles/2013/
- Windows安装PLSQL与配置：http://pygo2.top/articles/47618/

<font size=6.5 color='red'>部署系列持续更新中。。。。。。</font>
