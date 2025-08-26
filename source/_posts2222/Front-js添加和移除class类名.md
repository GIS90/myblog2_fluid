---
title: Javascript添加和移除Class类名
index_img: /img_index/index/20201013-001.png
categories:
  - [前端开发]
tags: [Javascript]
abbrlink: 27102
date: 2020-10-13 16:47:46
updated: 2020-10-13 16:47:46
desc: Javascript添加和移除html标签Class类名
keywords: Javascript, 前端, Class, 类名, 修改, 添加
---


#### 背景
{% note primary %}
最近再写一个综合系统，包含业务部分、日常任务管理、软件/模板管理等多种功能，前端使用的Adminlte样式，基于Bootstrap的。但是，写HTML经常会遇到添加和移除Class类名的情况，经常忘记，写个blog来记录。
{% endnote %}

<!--more-->

<hr />

#### 添加Class类名

> 方法一

一次只能设定一个类。
```
// 获取元素
element = document.getElementById('元素id');
element.className = '类名';

// 等同代码
document.getElementById('元素id').className = '类名';
```

> 方法二

用来设置自定义属性和值，如果是class，直接传入"class"即可；可以通过 .getAttribute 方法获取。
```
element = document.getElementById('元素id');
element.setAttribute('属性名','值');

// 等同代码
document.getElementById('元素id').setAttribute('属性名','值');
```

#### 移除Class类名

移除的方法与添加的方式都是对应的。

> 方法一

```
// 获取元素
element = document.getElementById('元素id');
element.className = '';

// 等同代码
document.getElementById('元素id').className = '';
```

> 方法二

直接通过removeAttribute移除。
```
element = document.getElementById('元素id');
element.removeAttribute('属性名');

// 等同代码
document.getElementById('元素id').removeAttribute('属性名');
```
