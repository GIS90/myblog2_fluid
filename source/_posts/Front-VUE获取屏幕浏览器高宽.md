---
title: JavaScript获取屏幕浏览器高宽
index_img: /img_index/index/20220509-001.png
categories:
  - [前端开发]
tags: [VUE, JavaScript]
abbrlink: 56199
date: 2022-05-09 20:08:26
updated: 2022-05-09 20:08:26
desc: JavaScript获取屏幕浏览器高宽
keywords: VUE, JavaScript
---



{% note info %}
对于写前端代码的，经常会计算一些css样式，获取屏幕浏览器高宽也是家庭便饭，记录一下。
{% endnote %}

{% label info@Hashcat %}

<!--more-->
<hr />

全局变量，直接上代码：
```
// 浏览器可用高度、宽度
document.body.scrollHeight
document.body.scrollWidth

// HTML页面的可用高度、宽度
document.body.clientHeight
document.body.clientWidth
```

学习参考：
- https://www.jianshu.com/p/e324cbd5ed3d
- https://blog.csdn.net/grkcsdn/article/details/118556380
- https://www.jb51.net/article/106472.htm
