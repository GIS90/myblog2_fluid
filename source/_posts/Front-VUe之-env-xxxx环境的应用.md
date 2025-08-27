---
title: VUE之.env.xxxx环境的应用
index_img: /img_index/index/20220309-001.jpg
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 55138
date: 2022-03-09 19:34:34
updated: 2022-03-09 19:34:34
desc: VUE之.env.xxxx环境的应用
keywords: VUE, env, development, production, 配置文件
---



最近一段时间在写VUE前端项目，发现有.env.development、.env.production，于是查了下官网，学习总结一下。


<!--more-->
<hr />


- .env 全局默认配置文件，不论什么环境都会加载合并

- .env.development 开发环境下的配置文件

- .env.production 生产环境下的配置文件

加载顺序：.env -> .env.development  或者 .env -> .env.production
后续加载配置文件中的变量会覆盖.env环境中同名的变量，而且上述的文件中可以添加变量，添加的变量在项目中可以通过process.env.XXXXX获取。

注意：
{% note warning %}
属性名必须以VUE_APP_开头，比如VUE_APP_XXX
{% endnote %}

开发环境.env.development文件示例：
```
# base
NODE_ENV = 'development'
PORT = 10002

# api
VUE_APP_BASE_API = 'http://127.0.0.1:9999/'
VUE_APP_AVATAR_API = '/user/avatar'
```

生产环境.env.production文件示例：
```
# base
NODE_ENV = 'production'
PORT = 10001

# api
VUE_APP_BASE_API = 'http://121.4.56.169:9999/'
VUE_APP_AVATAR_API = '/user/avatar'
```

官网详细介绍：https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F

<font size=5.5 color='red'>***坚持每天学习。。。。。。***</font>
