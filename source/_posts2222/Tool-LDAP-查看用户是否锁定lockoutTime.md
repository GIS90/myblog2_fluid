---
title: LDAP查看用户是否锁定lockoutTime
index_img: /img_index/index/20190423-001.jpg
desc: 用python脚本来查看LDAP用户是否锁定，关键对象属性：lockoutTime
categories:
  - [工具集]
  - [Python]
tags: [LDAP, Python]
keywords: LDAP, lockoutTime, 锁定, python, 属性
abbrlink: 35151
date: 2019-04-23 10:57:01
updated: 2019-04-23 10:57:01
---

### 问题描述

    LDAP查看AD账号是否锁定

LDAP（Lightweight Directory Access Protocol）是轻量目录访问协议，一般都简称为LDAP。

<!--more-->
<hr />

### 解决方案

工作上对此颇有涉及，某天在解决bug时，发现账号是否锁定与错误密码次数对不上，查看别人的代码，哈哈哈，果然有个坑，查看用户账号是否锁定是通过查询域下所有机子的错误密码次数，计算账户是否锁定，我们设置的是5次，超过这个限制账号就会被锁定。

针对账号是否锁定，居然是否次数算出来的，而不是属性获取。。。如果真的是这样，个人觉得LDAP那就太傻了。上网查了一下，果然有用户属性直接判断账号是否被锁定，那就是***lockoutTime***属性。没错，它也是解锁的属性。看官方解释：

![官网ldap_lockoutTime](ldap_lockoutTime.png)

最重要就是划红线的那2句话，大致意思lockoutTime的值是一个整形数值，并且等于***0***的时候，账号不被锁定。推理出来就是不为***0***，账号就会被认为锁定。

lockoutTime官方：https://ldapwiki.com/wiki/Lockouttime
