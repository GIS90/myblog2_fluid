---
title: 'JAVA的Error:loading:xxxx\jvm.dll问题解决'
index_img: /img_index/index/20210718-001.jpg
categories:
  - 服务器
tags:
  - Java
  - ERROR集
desc: '关于Error:loading:xxxx\jvm.dll的问题解决'
keywords: 'java, Error, loading, jvm.dll'
abbrlink: 35460
date: 2021-07-18 11:22:52
updated: 2021-07-18 11:22:52
---


### 1、问题描述


在服务器执行java环境的时候，发现报以上的错误：<font color="red" size="5">***Error: loading: xxxx\jvm.dll***</font>，记得在服务器坏之前还是好的，修复了之后就变成这样了，不管咋样，修复好才是王道。

<!--more-->
<hr />

### 2、解决方案

直接修改path环境变量，把<font color="red" size="5">***%JAVA_HOME%/bin;%JAVA_HOME%/jre/bin;***</font>放到path变量的最前面。

> 解决过程

- 首先，报***Error: loading: xxxx\jvm.dll***这个文件的问题，正常java的环境应该回去找安装好的jvm.dll文件。
- 于是，我把这个对应指定的文件重命名，发现报另一个的错误：***Error: missing `server‘ JVM at `XXXXXbin\server\jvm.dll‘.***
- 初步设想应该是JAVA环境配置出了问题，于是把JAVA环境重新配置了一边，好了。

### 3、原因

应该是JAVA去找jvm.dll的时候，从PATH的顺序去找，把JAVA配置的路径放在最前面，先从JAVA环境找起。
