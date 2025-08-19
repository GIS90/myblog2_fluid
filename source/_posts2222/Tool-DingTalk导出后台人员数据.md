---
title: DingTalk后台导出人员数据
index_img: /img_index/index/20211126-001.jpeg
categories:
  - [工具集]
tags: [DingTalk]
abbrlink: 45420
date: 2021-11-26 23:35:00
updated: 2021-11-26 23:35:00
desc: DingTalk后台导出人员数据
keywords: DingTalk, 后台, 导出, 人员数据
---



{% note warning %}
程序推送消息需要2个参数，第一个是消息内容，第二个就是消息接受者，利用DingTalk OpenApi，传入的User ID是DingTalk的ID，具体获取方式请查看下列内容。
{% endnote %}

{% label primary@DingTalk %}

<!--more-->
<hr />

#### 1、登录DingTalk官网

官网地址：https://www.dingtalk.com/
以管理员的角色登录DingTalk，点击快捷入口的通讯录管理。
![](1.png)

#### 2、批量导入/导出/修改

选择批量导入/导出/修改功能。
![](2.png)

#### 3、导出数据

选择导出/修改成员功能，设置导出的员工，点击下载。
![](3.png)

#### 4、数据展示

导出的excel数据中，第一列为DingTalk User ID，需要结合表数据的第二列与绩效明细数据进行vlookup。
![](4.png)

#### 5、其他

- DingTalk（钉钉）创建企业内部机器人：http://pygo2.top/articles/32206/
- DingTalk后台导出人员数据：http://pygo2.top/articles/45420/
