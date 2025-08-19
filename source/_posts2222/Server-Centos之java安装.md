---
title: Contos7.5环境上的Java安装
index_img: /img_index/index/20210116-001.jpg
categories:
  - [服务器]
tags: [Linux, Java]
top: false
abbrlink: 50773
date: 2021-01-16 21:30:19
updated: 2021-01-16 21:30:19
desc: 基于Centos7.5服务器上的Java安装
keywords: Centos, Java, yum, install
---


#### 1、简述
{% note primary %}
基于云服务器Centos7.5，安装libreoffice需要java环境，安装方式有2种，建议选择第二种手动方式。首先，安装的路径你可以自动随便选择；其次yum安装的jdk只是jre环境，而手动安装包含了JDK与JRE2个环境，包含dt.jar、tools.jar包。
{% endnote %}

{% label danger@Linux %} {% label info@Java %}

<!--more-->
<hr />

前几天安装libreoffice环境，不清楚这个是干什么的，可以参考<a href="/articles/10477/" target="_blank" class="block_project_a">libreoffice安装与使用</a>。

#### 2、系统环境

Contos7.5

{% raw %}
<div class="post_cus_note">--------------- Let's go ---------------</div>
{% endraw %}

#### 3、安装

这个主要用到yum命令，不清楚使用的同学可以baidu一下。

> yum

- 查找JDK

    ```
    yum search jdk
    ```
    ![](yum_search.png)

- 安装

    这里我选择的是1.8版本，根据服务器的位数选择对应的版本。
    ```
    yum install java-1.8.0-openjdk.x86_64
    ```
    安装完是下面这个样子：
    ![](java_list.png)

- 配置

    打开/etc/profile文件，把下面配置加到最下面。
    ```
    #set java environment
    JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.275.b01-0.el7_9.x86_64
    PATH=$PATH:$JAVA_HOME/bin
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export JAVA_HOME CLASSPATH PATH
    ```

- 验证

    ```
    java -version
    ```
    ![](java_version.png)

- 其他说明

    yum安装的默认路径为：/usr/lib/jvm。

> 手动

- 下载源码包

    下载地址：https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
    选择需要安装的版本，这里选择的是：Linux x64 Compressed Archive。

- 上传服务器
    ```
    scp ~/Downloads/jdk-8u281-linux-x64.tar.gz root@121.4.56.169:~
    ```
- 解压

    ```
    tar -zxvf jdk-8u281-linux-x64.tar.gz
    mv jdk-8u281-linux-x64 /usr/local/jdk1.8.0_281
    ```

- 配置

    同样还是/etc/profile文件。
    ```
    #set java environment
    JAVA_HOME=/usr/local/jdk1.8.0_281
    JRE_HOME=/usr/local/jdk1.8.0_281/jre
    CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
    PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
    export JAVA_HOME JRE_HOME CLASS_PATH PATH
    ```

- 测试

    ```
    java -version
    ```

#### 4、 JDK与JRE区别


> 含义

- JDK：java development kit
- JRE：java runtime environment

> 组成

- JDK：JVM + 基础类库 + 开发工具
- JRE：JVM
