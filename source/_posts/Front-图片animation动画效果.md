---
title: 图片增加选中animation动画效果
index_img: /img_index/index/20230703-001.jpg
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 65418
date: 2023-07-03 20:54:21
updated: 2023-07-03 20:54:21
desc: 图片增加选中animation动画效果
keywords: VUE, animation, 动画
---



{% note danger %}
前几天用v-viewerjs写了一个头像设置页面，用css的animation给图片选中加了一个动画效果，记录一下animation的基础用法以及实现的代码。
{% endnote %}

{% label info@VUE %} {% label danger@animation %}  {% label default@动画 %}

<!--more-->
<hr />

上篇博客记录了图片的选择事件，其中通过ref去对img元素进行样式修改，***viewer-select-image***样式类就是图片选择的效果，通过animation实现图片的方法缩小，效果简简单单，但是瞬间高大上了许多，代码也不多。

#### 代码

```
@keyframes select-image-an {
  0% {
    border: 27px solid;
    border-image: linear-gradient(#FF0000FF, #84cdfa, #FFFF00FF) 1;
  }
  100% {
    border: 7px solid;
    border-image: linear-gradient(#FF0000FF, #84cdfa, #FFFF00FF) 1;
  }
}

.viewer-select-image {
  animation: select-image-an 1.5s linear infinite alternate;
}
```

#### 语法糖

***animation: select-image-an 1.5s linear infinite alternate;***
select-image-an：动画名称，与@keyframes select-image-an关联，其中0%是初始状态，100%为全状态，在0-100之间也可以在一些效果
1.5s：动画一次的持续时间
linear：动画运动轨迹的速度，linear为匀速，默认是ease（低俗->加速->减速）
infinite：动画播放次数，infinite无限次执行
alternate：往返动画

#### 相关学习

- 菜鸟：https://www.runoob.com/cssref/css3-pr-animation.html
- 度娘：一搜一大推，问什么度娘都会告诉你
