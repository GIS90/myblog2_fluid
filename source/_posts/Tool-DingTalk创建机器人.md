---
title: DingTalk（钉钉）创建企业内部机器人
index_img: /img_index/index/20211126-001.jpeg
categories:
  - [工具集]
tags: [DingTalk]
abbrlink: 32206
date: 2021-11-26 21:19:51
updated: 2021-11-26 21:19:51
desc: DingTalk（钉钉）创建企业内部机器人
keywords: DingTalk, 钉钉, 创建, 企业, 机器人
---


#### 背景

{% note primary %}
工作上的项目有个简单的需求，说有好的什么方式可以实现工资的明细可以自动化推送，经过沟通，打算把方向定位到钉钉上，并不是打广告啊，钉钉在中小型企业，甚至大企业使用程度还是很广的，于是加班加点近几天写了个简单的程序，功能就是调用钉钉openApi实现绩效工资明细自动化推动到行员钉钉。
{% endnote %}

{% label info@DingTalk %} {% label primary@机器人%}

<!--more-->
<hr />

### 正文
程序的基本功能已经开发完了，但是需要对DingTalk（钉钉）进行一些设置，本篇为讲述DingTalk创建机器人的相关操作，如果有想了解程序的可以看文章末尾的github地址，后续也会对这个程序写个详细的文章出来。


- #### 登录DingTalk官网
    以管理员的方式钉钉开发中后台，依次选择应用开发 > 企业内部开发 > 机器人，点击创建应用。
    官网地址：https://open-dev.dingtalk.com/
    ![](1.png)

- #### 配置机器人
    按要求填写应用名称、应用描述以及图标，创建完之后还是可以进行内容修改的。
    ![](2.png)

- #### 查看AppKey与AppSecret
    回到主页面选择刚创建的机器人，查看基本信息，记录机器人的AppKey与AppSecret，在程序的config.yaml配置文件中需要配置对应的参数。
    ![](3.png)

- #### 开通权限
    选择权限管理 > 机器人 > 企业内机器人发送消息权限，开通这个发消息的权限。
    ![](4.png)

- #### 发布机器人
    选择版本管理与发布，发布需要发消息的机器人。
    ![](5.png)

### 学习参考

* 企业自建单聊机器人（官网）：https://developers.dingtalk.com/document/robots/enterprise-created-chatbot
* 项目地址：https://github.com/GIS90/dtalk_send_pas
- DingTalk（钉钉）创建企业内部机器人：http://pygo2.top/articles/32206/
- DingTalk后台导出人员数据：http://pygo2.top/articles/45420/
