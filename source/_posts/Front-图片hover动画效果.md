---
title: 图片hover动画效果，另一种动画方式
index_img: /img_index/index/20230714-001.jpg
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 37930
date: 2023-07-14 00:09:32
updated: 2023-07-14 00:09:32
desc: 图片hover动画效果，另一种动画方式
keywords: VUE, transition, transform
---




用transition实现动画样式，也是css中实现动画的常用方式之一，本人用的最多的就是transition、animation，VUE还有自带的Transition组件，但是Transition是切换的效果。

{% label success@VUE %} {% label default@动画 %}


<!--more-->
<hr />




#### 实现代码

鼠标划过图片的时候有动画效果，直接上代码实现结果：
```
<style scoped>
.viewer-box {
  text-align: center;
}

.viewer-box-image {
  width: 120px;
  height: 120px;
  margin: 5px 6px 5px 6px;
  transition: transform .4s ease-in-out;
  transform-origin: center center; /* 中心点 */
}

.viewer-box-image:hover {
  transform: scale(1.2) rotate(360deg); /* 缩放+旋转 */
}
</style>
```

#### 语法糖

> transition: transform .4s ease-in-out;

- transition-property: none| all | property >>> all表示所有属性都有过渡效果，property定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔
- transition-duration: time值； >>> 默认是0 没有动画效果，以秒或者毫秒计
- transition-timing-function:linear|ease|ease-in|ease-out|ease-in-out >>> 动画效果

> transform: scale(1.2) rotate(360deg)

- transform: scale(1.2);  缩放
- transform: translate(10px, 10px);  平移
- transform: rotate(360deg);  旋转
- transform: skew(90deg, 10deg);  斜切skewX, skewY

{% raw %}
<div class="post_cus_note">注意</div>
{% endraw %}

关于transition，transform，translate三者的关系可以先搞清楚了，不然很容易蒙了。
