---
title: Element-UI【Form组件】unpected-width报错的解决
index_img: /img_index/index/20220329-001.jpg
categories:
  - [前端开发]
tags: [VUE, Element-UI]
abbrlink: 16084
date: 2022-03-29 21:21:15
updated: 2022-03-29 21:21:15
desc: Element-UI的tree组件子选父不选提交数据问题
keywords: VUE, Element-UI
---


{% note primary %}
在使用Element-UI的Form组件时候，发现控制台报了一个***Error：[ElementForm]unpected width***
{% endnote %}

{% label info@VUE %} {% label primary@Element-UI %}

<!--more-->
<hr />

报错：
```
vue.esm.js:1897 Error: [ElementForm]unpected width
    at VueComponent.getLabelWidthIndex (element-ui.common.js:23014)
    at VueComponent.deregisterLabelWidth (element-ui.common.js:23027)
    at VueComponent.updateLabelWidth (element-ui.common.js:23226)
    at VueComponent.beforeDestroy (element-ui.common.js:23253)
    at invokeWithErrorHandling (vue.esm.js:1863)
    at callHook (vue.esm.js:4222)
    at VueComponent.Vue.$destroy (vue.esm.js:3981)
    at destroy (vue.esm.js:3168)
    at invokeDestroyHook (vue.esm.js:6119)
    at invokeDestroyHook (vue.esm.js:6124)
```
使用***el-form***组件的时候，设置labelWidth设为auto时，如果el-form-item内容如果你用了v-show会提示上面的错误，可以v-if替换成v-show，每次重新渲染组件，这样就可以了。
