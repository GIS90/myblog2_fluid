---
title: VUE之scripts的学习
index_img: /img_index/index/20220313-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 20773
date: 2022-03-13 11:27:49
updated: 2022-03-13 11:27:49
desc: VUE之scripts的学习
keywords: VUE, scripts
---

学习记录以下关于vue script脚本命令的学习。

<!--more-->
<hr />


```
"scripts": {
  "dev": "vue-cli-service serve --open --mode development",
  "lint": "eslint --ext .js,.vue src",
  "build:prod": "vue-cli-service build --report",
  "build:dev": "vue-cli-service build --report --mode development",
  "preview": "node build/index.js --preview",
  "new": "plop",
  "svgo": "svgo -f src/icons/svg --config=src/icons/svgo.yml",
  "test:unit": "jest --clearCache && vue-cli-service test:unit",
  "test:ci": "npm run lint && npm run test:unit"
},
```

> dev

开发模式启动we服务
> lint

检查代码是否符合eslint语法标准
> build:prod

打包生产环境
> build:dev

打包开发环境
> new

新建一个view/router/store，pop是一个插件，感觉挺不错的
> test:unit

单元测试
> test:ci

语法检查 + 单元测试

<font size=5.5 color='red'>***坚持每天学习。。。。。。***</font>
