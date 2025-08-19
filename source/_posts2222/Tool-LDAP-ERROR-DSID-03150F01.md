---
title: 'LDAP-ERROR:DSID-03150F01'
index_img: /img_index/index/20180319-001.png
desc: 在处理LDAP的时候遇到的问题：LDAP-ERROR, DSID-03150F01
categories:
  - [工具集]
tags: [LDAP, ERROR集]
keywords: LDAP, ERROR, LDAP-ERROR, DSID-03150F01
abbrlink: 13042
date: 2018-03-19 19:45:26
updated: 2018-03-19 19:45:26
---

#### 1、问题描述：

在负责的项目中，有个跟LDAP打交道的，其中有个任务就是把人添加到指定的CN下。某天hr来找，说群发的邮件部分人没有收到。于是找到这个任务脚本，打开查看日志，居然报下面的错误。

<!--more-->
<hr />

```
{
    'info': '000021B1:
    SvcErr: DSID-03150F01,
    problem 5005 (UNABLE_TO_PROCEED),
    data 0\n',
    'desc': 'Operations error'
}
```

#### 2、解决方案

第一思维先google了一下，发现这个错误的少之又少，并且查看后没有解决方案。后来通过查看日志，发现这个错误有一个月之前就有了。但是这个任务在beta环境还是正常执行的，于是比对线上与beta环境的不同之处，任务代码一模一样，LDAP环境也保持一致。执行的发现把员工加到执行CN下的员工数不一样。于是把线上获取的数据量改小了一些，正常了。那么问题就定位到数据大小的问题决定着错误，说明错误指向了LDAP服务器，但是线上与beta的DLAP环境，配置是一样的啊。
把线上的数据源数据同步到beta环境，执行beta环境的任务脚本，一样的错误。。。一样，哈哈那就确定了，是LDAP上的问题，去官方查询相关的信息。果然有一些配置有决定性的作用，打开注册表编辑器，创建以下路径键值&数值：

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]
    "Maximum Audit Queue Size"=dword:000088b8

![mountain](mountain.png)

#### 3、建议
***<font color="#dd0000" size="5">遇到问题莫慌，最重要的是找到问题根源，只要找到问题的根源，才是解决问题的王道！！！</font>***
