---
title: Window Server 2012R2添加我的电脑
index_img: /img_index/index/20230203-001.png
categories:
  - Windows
tags:
  - Window Server
desc: Window Server 2012添加我的电脑
keywords: 'Window, Server, 2012, R2, 添加, 我的电脑'
abbrlink: 7239
date: 2023-02-03 12:28:36
updated: 2023-02-03 12:28:36
---



{% note primary %}
新服务器刚装的Window Server 2012R2系统，打开服务器发现找不到我的电脑，需要对其进行简单操作下才可以显示。
{% endnote %}

<!--more-->

<hr />

#### 系统版本

|    名称    |              版本              |
|:----------:|:------------------------------:|
| 服务器系统 | Windows Server2012 R2 Standard |

#### 我的电脑

1、运行win徽标键+R，输入下列代码：
```
rundll32.exe shell32.dll,Control_RunDLL desk.cpl,,0
```

<font size=6.5 color='red'>逗号一定要用英文逗号,</font>
<font size=6.5 color='blue'>逗号一定要用英文逗号,</font>
<font size=6.5 color='yellow'>逗号一定要用英文逗号,</font>

2、确定之后，会出现以下设置配置的项目，勾选我的电脑以及需要桌面显示快捷入口。
![](computer.jpeg)
