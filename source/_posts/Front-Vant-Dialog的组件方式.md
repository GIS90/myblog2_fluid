---
title: Vant-Dialog的组件调用方式的注意要点
index_img: /img_index/index/20211123-001.png
categories:
  - [前端开发]
tags: [VUE, vant]
abbrlink: 50291
date: 2021-11-23 22:53:41
updated: 2021-11-23 22:53:41
desc:
keywords:
---


在学习VUE的过程中，使用了移动UI的vant进行实战，在调用Dialog组件的时候，直接在methdod方法中使用没什么问题，但是把vant-dialog用在template标签中，控制台会报出警告，如下：
```
[Vue warn]: Extraneous non-props attributes (show, title, show-cancel-button) were passed to component but could not be automatically inherited because component renders fragment or text root nodes.
at <Dialog show=false onUpdate:show=fn title="新增" ... >
at <Type onVnodeUnmounted=fn ref=Ref< undefined > >
```


<!--more-->
<hr />

vant官网提出了dialog的调用方式，函数调用与组件调用。在组件调用方式的时候报以上警告，而且Dialog会直接显示出来，在组件调用的时候需要注意：
```
[Dialog.Component.name]: Dialog.Component,
或者
van-dialog: Dialog.Component,
```
官网以及vant github都有解决的方案。
