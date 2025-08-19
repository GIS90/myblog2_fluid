---
title: Hexo-环境搭建之node版本问题
index_img: /img_index/index/20191217-001.png
categories:
    - [Hexo]
tags: [Hexo]
abbrlink: 10512
date: 2019-12-17 10:55:42
updated: 2019-12-17 10:55:42
desc: 在云服务搭建hexo博客环境出现的问题，原因是node版本过低引发的血案
keywords: hexo, next, Hexo, 博客, blog, node, nodejs, Unexpected identifier, usr/lib/node_modules/hexo-cli/node_modules/chokidar/index.js:150
---

#### 背景

{% note success %}
昨天买了个腾讯云服务器 + 域名，于是把自己的blog迁移云服务上，就得搭建nodejs、npm、hexo等运行环境。但是环境都装好了，一运行***hexo***居然报错了，错的内容在正文。

既然出现错误，解决就好了。
{% endnote %}

<!--more-->
<hr />

#### 错误内容
```
usr/lib/node_modules/hexo-cli/node_modules/chokidar/index.js:150
async remove(item) {
^^^^^^


SyntaxError: Unexpected identifier
at createScript (vm.js:56:10)
at Object.runInThisContext (vm.js:97:10)
at Module._compile (module.js:549:28)
at Object.Module._extensions..js (module.js:586:10)
at Module.load (module.js:494:32)
at tryModuleLoad (module.js:453:12)
at Function.Module._load (module.js:445:3)
at Module.require (module.js:504:17)
at require (internal/module.js:20:19)
at Object.<anonymous> (/usr/lib/node_modules/hexo-cli/node_modules/hexo-fs/lib/fs.js:6:18)
at Module._compile (module.js:577:32)
at Object.Module._extensions..js (module.js:586:10)
at Module.load (module.js:494:32)
at tryModuleLoad (module.js:453:12)
at Function.Module._load (module.js:445:3)
at Module.require (module.js:504:17)
```

上述的错误搜索了好半天才没有到找到一个解决办法，不过在失去希望的时候，发了在baidu论坛有个人跟我报了一样的错误，而且还解决了，不买官司了，原因是：<font size=6.5 color='red'>node版本过低</font>。

#### 解决办法

升级nodejs即可，具体操作如下：

```
npm install -g n

# 升级到最新稳定版本：
n stable

# 升级到最新版本：
n latest

# 升级到指定版本：
n 0.10.26
```


#### 学习参考

n：https://www.jianshu.com/p/a2ee8f61a8ca
帖子：https://tieba.baidu.com/p/6311927039?red_tag=1232994946
