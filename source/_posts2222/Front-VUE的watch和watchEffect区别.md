---
title: VUE3的watch和watchEffect区别
index_img: /img_index/index/20230724-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 58210
date: 2023-07-24 22:23:54
updated: 2023-07-24 22:23:54
desc: VUE3的watch和watchEffect区别
keywords: VUE, watch, watchEffect
---


{% label warning@VUE %} {% label info@监听器 %}

<!--more-->
<hr />

最近官网学习了VUE3基础教程，跟VUE2比起来，改变还是挺大的，不过好多也很多，性能、开发效率，新增的组合式API很喜欢，也算下一步搞一个VUE3项目练练手，学完了不写，几天就生疏了。
不过学习的过程中发现VUE3新增了一个watchEffect，具体分别如下：
- **watch**和**watchEffect**都能监听响应式数据的变化，不同的是它们监听数据变化的方式不同。
- **watch**会明确监听某一个响应数据，而**watchEffect**则是隐式的监听回调函数中响应数据。
- **watch**在响应数据初始化时是不会执行回调函数的，**watchEffect**在响应数据初始化时就会立即执行回调函数。

watchEffect更好用。

<font color="red" size="6">坚持学习。。。。。。</font>
