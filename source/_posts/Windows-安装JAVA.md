---
title: Windows Server2012R2安装JAVA
index_img: /img_index/index/20220712-001.jpeg
categories:
  - [服务器]
tags: [Java, 部署教程系列]
abbrlink: 1151
date: 2022-07-12 23:35:44
updated: 2022-07-12 23:35:44
desc: Windows Server2012R2安装JAVA
keywords: Windows, Server, 2012, R2, 安装, JAVA
---



实战部署系列教程：<font color="purple" size="7">Windows安装JAVA1.7与配置</font>。

{% label default@Java %} {% label info@Windows %}

<!--more-->
<hr />

#### 版本说明

|    名称    |              版本              |
|:----------:|:------------------------------:|
| 服务器系统 | Windows Server2012 R2 Standard |
|    Java    |              1.7               |



#### 安装

1、打开Java下载地址，找到Java7SE（标准版），进行下载：
https://www.oracle.com/java/technologies/javase/javase7-archive-downloads.html
Windows版本，下载的时候根据安装系统选择对应的版本、位数：
![](511662565784_.pic.jpg)

2、下载好之后，直接copy到服务器，进行安装。
建议：安装的路径选择英文路径。
安装过程很简单，安装后截图：
![](521662565839_.pic.jpg)

3、环境变量配置
安装以后需要进行环境变量配置。
3.1 我的电脑（右键）->选择属性->高级系统设置->环境变量。
![](531662565857_.pic.jpg)

3.2 环境变量配置
3.3.1 新建JAVA_HOME变量
变量名：JAVA_HOME
变量值：JDK安装的绝对路径（D:\Program Files\Java\jdk1.7.0_17）
![](541662565884_.pic.jpg)

3.3.2 新建CLASSPATH变量
变量名：CLASSPATH
变量值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
变量值前面是.【英文点】
变量后面是;【英文分号】
![](551662565901_.pic.jpg)

3.3.3 修改Path变量
第三步：
新建两条路径:
;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
![](561662565918_.pic.jpg)

4 测试
打开cmd窗口（win+R），输入java，如果是以下提示安装成功，否则就是环境变量配置问题。
![](571662565933_.pic.jpg)



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
