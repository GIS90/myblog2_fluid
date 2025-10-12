---
title: Qunar项目之总结篇
index_img: /img_index/index/20190810-002.png
categories:
  - [Python]
tags: [Python]
abbrlink: 30296
date: 2019-08-10 20:10:44
updated: 2019-08-19 20:10:44
desc: 记录在Qunar所做过的项目，主要是python web相关的项目
keywords: web, python, qunar, 项目, 项目Outage, 项目Wwwhr, 项目Isapi, 项目Backyard, 项目New_hrtools, 项目Officedb, 项目Opsapp, 项目Opsbi, 项目Smsd, 项目Newoa_api, 项目Psapproval
sticky: 1001
---
{% note primary %}
Qunar负责的项目汇总。

<font color=#1E90FF size=8 face="黑体"> 感谢Qunar让我成长 ！！！</font>
{% endnote %}

<!-- more -->

<hr />
{% label danger@项目总结 %} {% label primary@Qunar %} {% label success@Python实战 %} {% label info@Web %}
<hr />

### 1、背景

统计一下负责项目的说明。

### 2、统计

| ID |   项目名称   |      中文名称      | 重要程度 | 技术架构                         |
| :-: | :-----------: | :-----------------: | :------: | :------------------------------- |
| 1 |   backyard   |       骆驼帮       |  ★★★  | 前后端分离项目【Python + React】 |
| 2 |     isapi     |     员工信息API     |  ★★★  | rest api，Python编写             |
| 3 |  new_hrtools  |   AD账号管理系统   |   ★★   | 任务脚本较多，Python编写         |
| 4 |   officedb   |  办公大厦管理系统  |    ★    | 后台【Flask】+ 模板              |
| 5 |    opsapp    |     Qtalk Apis     |   ★★   | rest api                         |
| 6 |    outage    |      故障系统      |  ★★★  | 后台【Flask】+ 前端【angularjs】 |
| 7 |  psapproval  |    qtalk审批申请    |   ★★   | rest api，Python编写             |
| 8 |     wwwhr     |  员工信息采集系统  |   ★★   | 后台【Flask】+ 模板              |
| 9 |   newoa_api   |    newoa审批Api    |   ★★   | rest api，Python编写             |
| 10 |     opsbi     | Ops故障可视化bi系统 |    ★    | 后台【Flask】+ 前端【angularjs】 |
| 11 |     smsdb     |       短信狗       |   ★★   | 软件 + 硬件                      |
| 12 | opstools-cron |      定时任务      |   ★★   | Node                             |

### 3、项目说明

#### 3.1、Isapi

> <font size=4.5 color='red'>简述</font>

Isapi是一个提供qunar员工信息数据的api项目，主要针对的用户群体是内部员工。数据来源于ehr系统（peoplesoft，简称：ps），ps使用的是Oracle数据库，基于ps提供的view试图，把员工数据同步到一个新的数据库，Isapi选择使用了postgresql，把员工信息数据封装json，通过http api接口的方式开放给用户，不仅解决了其他系统使用员工信息数据的问题，还有让ps数据隔离，提高了安全性、可用性。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/7285/" target="_blank" class="block_project_a">Qunar项目之Isapi-员工信息apis</a> -->

#### 3.2、backyard

> <font size=4.5 color='red'>简述</font>

用于Qunar员工内部使用的一个web内网系统，主要用于员工信息查询、考勤、公司相关的制度、条例等信息，是一个员工访问量相对较大的一个web系统。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/39002/" target="_blank" class="block_project_a">Qunar项目之Backyard-骆驼帮</a> -->

#### 3.3、new_hrtools

> <font size=4.5 color='red'>简述</font>

项目主要用于员工ad账号相关的处理，主要有API接口和定时任务2大部分组成，其中定时任务是重点，与ldap、数据库均有交互，是一样日常运维工作较多的一个项目。

<!-- > <font size=4.5 color='red'>详情</font> 

    <a href="/articles/61124/" target="_blank" class="block_project_a">Qunar项目之New-hrtools【AD账号管理系统】</a> -->

#### 3.4、officedb

> <font size=4.5 color='red'>简述</font>

Officedb系统是Qunar行政管理员工办公大厦的一个内部系统，主要针对于Qunar行政、QunarIT对办公大厦进行费用、员工信息等管理，实现Qunar办公大厦的自动化。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/47129/" target="_blank" class="block_project_a">Qunar项目之Officedb-办公大厦管理系统</a> -->

#### 3.5、opsapp

> <font size=4.5 color='red'>简述</font>

opsapp跟Isapi实现的功能大致一样，这个项目是专门用来提供给qtalk移动端在外网情况下进行使用的一个api接口，这个接口实现了访问内网需要的api接口，实现了在非内网情况使用qtalk功能。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/9363/" target="_blank" class="block_project_a">Qunar项目之Opsapp-QtalkApis</a> -->

#### 3.6、outage

> <font size=4.5 color='red'>简述</font>

主要用于用户进行报故障所用，主要有故障通报、故障review、故障产品线、对外API4大部分。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/26073/" target="_blank" class="block_project_a">Qunar项目之Outage-故障系统</a> -->

#### 3.7、psapproval

> <font size=4.5 color='red'>简述</font>

主要用于ehr系统部分流程审批、申请在qtalk移动端进行操作。ehr系统主要用于管理员工、员工使用的一个人事系统，只能通过pc端进行操作，为了能在qtalk移动端也同样进行操作，psapproval接入ehr的员工使用流程。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/204/" target="_blank" class="block_project_a">Qunar项目之Psapproval-Qtalk审批申请</a> -->

#### 3.8、wwwhr

> <font size=4.5 color='red'>简述</font>

用于采集入职Qunar员工基本个人信息的一个外部系统，信息主要用于ehr系统使用，是一个正常入职Qunar员工必须走的一步。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/3146/" target="_blank" class="block_project_a">Qunar项目之Wwwhr-信息采集系统</a> -->

#### 3.9、newoa_api

> <font size=4.5 color='red'>简述</font>

newoa_api是专门针对于PC端Newoa表单在移动端详情展示，主要用于数据格式化。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/34271/" target="_blank" class="block_project_a">Qunar项目之Newoa-api【Newoa审批Api】</a> -->

#### 3.10、opsbi

> <font size=4.5 color='red'>简述</font>

Opsbi是针对于Ops组造成故障的数据做的一个可视化web系统，数据源来源于Outage故障系统，是一个前端【angularjs + echart】+ 后台【python】一体的项目。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/63144/" target="_blank" class="block_project_a">Qunar项目之Opsbi-Ops故障可视化bi系统</a> -->

#### 3.11、smsdb

> <font size=4.5 color='red'>简述</font>

smsd是一个针对于机票事业部短信收发做的一个项目，实现了机票部门短信接收、发送、代收等功能。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/22299/" target="_blank" class="block_project_a">Qunar项目之Smsdb-短信狗</a> -->

#### 3.12、opstools-cron

> <font size=4.5 color='red'>简述</font>

Opstools-cron项目主要针对于其他项目有定时任务写的一个请求request crontab，由pm2进行管理。

<!-- > <font size=4.5 color='red'>详情</font>

    <a href="/articles/9777/" target="_blank" class="block_project_a">Qunar项目之Opstools-cron【定时任务】</a> -->

### 4、发布

#### 4.1、 提交代码

```
1 git add .

2 git commit -m "XXXX"

3 git tag q-xxxxxxxx-xx

4 git push origin master --tags
```

#### 4.2、自动化发布

1 打开自动化发布url。

2 左侧查看通过tag触发构建的项目，等待构建完成。如果构建过程中出现问题，请联系相关负责人进行处理。

3 点击Qunar Build，选择QA【beta环境】，先进行beta环境发布，参数详解如下：

    - server_list_group：A
    
    - tag_name：q-xxxxxxxx-xx
    
    - server_list_group代表发布机器组，发哪个组写哪个组名即可，全部写ALL。
    
    - tag_name为构建的tag名称。

  4 QA发布完成环境之后。

  5 在选择OPS【线上环境】进行发布，建议发布一台检查一台的方式去进行发现线上环境。

### 5、项目启动

#### 5.1、常规

```
(sudo) python tools/install_venv.py

(sudo)  tools/with_venv.sh python setup.py develop                  # develop 软链

(sudo)  tools/with_venv.sh main --config-file 配置文件 -v
```

#### 5.2、调试

```
(sudo) virtualenv .venv

(sudo) source .venv/bin/activate

(sudo) sudo pip install -r requirements.txt -i 源地址 --trusted-host 源地址域名

(sudo) tools/with_venv.sh python setup.py install                  # install 复制

(sudo) tools/with_venv.sh main --config-file 配置文件 -v
```

### 6、supervisorctl管理

登录服务器上切换到sudo用户，执行supervisorctl：

![](qunar_supervisorctl.png)

常用命令说明：

- status 查看项目运行状态
- restart outage-www 重启outage-www 项目
- start outage-www 启动outage-www 项目
- stop outage-www 停止outage-www项目
- start all 启动所有项目
- stop all停止所有项目
- tail -f outage-www 动态的查看outage-www项目日志
