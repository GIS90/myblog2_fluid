---
title: ES6之扩展运算符三个点
index_img: /img_index/index/20220120-001.jpg
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 3828
date: 2022-01-20 18:50:58
updated: 2022-01-20 18:50:58
desc: ES6之扩展运算符三个点
keywords: ES6
---

最近也写了不少的VUE的代码，发现...这个新的运算符用的还是比较多的，总结记录下。
总结一句话：
<font size=6.5 color='red'>把参数对象中可以遍历的属性都取出来放在新的对象中，实现多个对象扁平化。</font>


{% label primary@VUE %} {% label info@ES6 %}

<!--more-->
<hr />


不多说，直接看例子：
```
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];
console.log([...arr1, ...arr2, ...arr3])

# 结果：
[ 'a', 'b', 'c', 'd', 'e' ]
```
以此类推，开发中最常用就是把abc字符串等数据换成Object对象的数据，不过记住原理，万变不离其宗。
再来个对象的例子加深下印象：
```
let obj1 = { name: 'mingliang.gao' }
let obj2 = { sex: 'man' }
let obj = { ...obj1, ...obj2 }
console.log(obj)

# 结果：
{ name: 'mingliang.gao', sex: 'man' }
```

学习参考：https://es6.ruanyifeng.com/#docs/array
