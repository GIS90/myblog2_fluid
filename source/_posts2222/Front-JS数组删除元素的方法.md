---
title: Jquery数组删除元素的方法
index_img: /img_index/index/20211118-001.jpg
categories:
  - [前端开发]
tags: [Javascript]
abbrlink: 43302
date: 2021-11-18 00:05:51
updated: 2021-11-18 00:05:51
desc: 总结一下jquery数组删除元素的方法
keywords: jquery, 数组, 删除
---

{% note primary %}
写项目过程中经常写前端js，记录一下删除数组中指定某个元素的常用方法。
{% endnote %}


<!--more-->
<hr />

> 测试数据

```
var arr = ['a','b','c','d'];
```

#### 方法一

<font color="red" size="5.5">**最常用的方法**</font>
```
var index = arr.indexOf('c')
if(index > -1){
    var el = arr.splice(index, 1)
    console.log(el)
}
```
结果：
```
[ 'a' ]
[ 'b', 'c', 'd' ]
```
说明：
splice参数有多个，第一个代表删除元素的位置；第二个为删除的个数。

#### 方法二
```
var index = arr.indexOf('c')
if(index > -1){
    delete arr[index]
    console.log(arr)
}
```
结果：
```
[ 'a', 'b', , 'd' ]
```
说明：
使用delete删除元素之后数组长度不变，只是被删除元素变为""了。

#### 方法三
```
var el = arr.pop()
console.log(el)
console.log(arr)
```
结果：
```
d
[ 'a', 'b', 'c' ]
```
说明：
pop方法用于删除数组中的最后一项，并且数据返回到新的变量。

#### 方法四
```
var el = arr.shift()
console.log(el)
console.log(arr)
```
结果：
```
a
[ 'b', 'c', 'd' ]
```
说明：
shift方法用于删除数组中的第一项，并且数据返回到新的变量。

最常用的数据删除元素方法为splice，用indexOf去获取删除元素的位置。
