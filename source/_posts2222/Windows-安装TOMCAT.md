---
title: Windows Server2012R2安装TOMCAT
index_img: /img_index/index/20220721-001.jpg
categories:
  - [服务器]
tags: [Tomcat, 部署教程系列]
abbrlink: 2013
date: 2022-07-21 23:15:51
updated: 2022-07-21 23:15:51
desc: Windows Server2012R2安装TOMCAT
keywords: Windows, Server, 2012, R2, 安装, TOMCAT
---


实战部署系列教程：<font color="#dd0000" size="7">Windows安装Tomcat7与配置</font>。



<!--more-->
<hr />

{% label primary@Tomcat %} {% label info@Windows %}

#### 版本说明

|    名称    |              版本              |
|:----------:|:------------------------------:|
| 服务器系统 | Windows Server2012 R2 Standard |
|   Tomcat   |               7                |


#### 安装

1、打开Tomcat下载地址，根据项目选择对应的版本进行下载，本文选择Tomcat7：
https://archive.apache.org/dist/tomcat/
![](581662566099_.pic.jpg)

2、选择bin，进入之后会有各种版本，本文选择7.0.99最后一个7版本
![](591662566115_.pic.jpg)

3、Windows版本，下载的时候根据安装系统选择对应的版本、位数，本文选择了：apache-tomcat-7.0.99-windows-x64.zip
这种包的好处在于不用安装，直接能用，也可以选择exe安装版本。
![](601662566140_.pic.jpg)

4、下载好之后，直接copy到服务器，直接解压放在D盘根目录。
建议：安装的路径选择英文路径。
![](611662566156_.pic.jpg)

5、环境变量配置
安装以后需要进行环境变量配置。
5.1 我的电脑（右键）->选择属性->高级系统设置->环境变量。
![](621662566171_.pic.jpg)

5.2 环境变量配置
5.2.1 新建CATALINA_HOME变量
变量名：CATALINA_HOME
变量值：Tomcat安装的绝对路径（D:\apache-tomcat-7.0.99）
![](631662566186_.pic.jpg)

5.2.2 修改Path变量
PATH变量在末尾添加：;%CATALINA_HOME%\bin

6 测试变量
打开cmd窗口（win+R），输入echo %CATALINA_HOME%，如果是Tomcat安装路径，否则就是环境变量配置问题。
![](641662566202_.pic.jpg)


7 启动Tomcat
7.1 进入到Tomcat安装路径下的bin目录
7.2 shift + 右键->选择->在此文件夹打开命令窗口
![](651662566218_.pic.jpg)

7.3 输入startup.bat，启动tomcat
![](661662566262_.pic.jpg)

7.4 打开浏览器：127.0.0.7:8080，出现tomcat服务
![](671662566275_.pic.jpg)


#### 问题

> 1、cmd控制台乱码

1.1 Tomcat安装路径 -> conf -> logging.properties
![](681662566322_.pic.jpg)
修改以下内容：
```
# java.util.logging.ConsoleHandler.encoding = UTF-8
java.util.logging.ConsoleHandler.encoding = CBK
```

1.2 重启Tomcat，解决乱码问题
乱码解析：
Cmd控制台编码与Tomcat日志编码不一致，查看cmd控制台编码：
![](691662566345_.pic.jpg)
cmd控制台为GBK编码，Tomcat日志编码默认UTF-8编码，统一编码就解决了


> 2、Tomcat修改内存

在部署PSA4.3.4系统运行后，发现系统Tomcat控制台报错：
java.lang.OutOfMemoryError：PermGen space，意思就是内存不足，
由于Windows服务器主要用来做应用服务所用，所以手动分配Tomcat内存大小。

2.1 Windows找到D:\apache-tomcat-7.0.99\bin下的catalina.bat文件
（Linux：D:\apache-tomcat-7.0.99\bin\catalina.sh文件）
![](701662566387_.pic.jpg)

2.2 用notepad打开，在111行setlocal后面加上：
```
set JAVA_OPTS=%JAVA_OPTS% -server -XX:PermSize=8192M -XX:MaxPermSize=8192M
8G
```
服务器总大小为32G，就一个tomcat，分的大了点，根据服务器项目情况可以适当调整。
不过，Xms与Xmx参数之间的差值建议不要太大，不然会造成内存浪费，这里设置相等。
JAVA_OPTS配置文件上面搜索JAVA_OPTS，有说明。
![](711662566416_.pic.jpg)

2.3 重启Tomcat解决OutOfMemoryError



{% raw %}
<div class="post_cus_note"> =========== END =========== </div>
{% endraw %}

#### 部署教程系列

- LINUX+ORACLE部署安装：http://pygo2.top/articles/40987/
- Linux安装JAVA1.7与配置：http://pygo2.top/articles/13502/
- Windows安装JAVA1.7与配置：http://pygo2.top/articles/1151/
- Windows安装Tomcat7与配置：http://pygo2.top/articles/2013/
- Windows安装PLSQL与配置：http://pygo2.top/articles/47618/

<font size=6.5 color='red'>部署系列持续更新中。。。。。。</font>
