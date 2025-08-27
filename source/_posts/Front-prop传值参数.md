---
title: VUE之Props传值参数
index_img: /img_index/index/20220418-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 52857
date: 2022-04-18 14:35:14
updated: 2022-04-18 14:35:14
desc: VUE之Props传值参数
keywords: VUE, PROPS
---


{% label info@总结一下VUE关于Props的传值 %}

<!--more-->
<hr />

#### 1、定义

- 用于父组件对子组件之间的值传递，属于单向传递（父组件->子组件），不可逆。
- 如果对父组件传递过来的值进行赋值，会报错，子组件如想返回父组件传递值，可以用emit。

#### 2、用途

如定义，就是父组件->子组件的值传递。


#### 3、语法糖

父组件：
```
<excel-merge-opr
  :show="mergeDialogStatus"
  :list="selectList"
  @close-file-merge="closeFileMerge"
/>
```

子组件：
```
props: {
  show: {
    type: Boolean,
    require: true,
    default: false,
    validator(value) {
      return [true, false].includes(value)
    }
  },
  list: {
    type: Array,
    require: true,
    default: function() {}
  }
}
```

- type
数据类型，也就是JS的数据类型：
String
Number
Boolean
Array
Object
Date
Function
Symbol

- require
是否必须传入，可选值：true false

- default
默认值

- validator
传入值校验

#### 4、学习参考

官网API：https://v3.cn.vuejs.org/guide/component-props.html#prop-%E7%B1%BB%E5%9E%8B
