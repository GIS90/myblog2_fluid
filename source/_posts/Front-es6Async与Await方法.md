---
title: ES6Async与Await方法的学习
index_img: /img_index/index/20220218-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 17064
date: 2022-02-18 19:39:37
updated: 2022-02-18 19:39:37
desc:
keywords: Async, Await
---

#### 定义

很简单，一句话：
async用于异步方法，await用于等待异步方法执行完成。

所以，await必须搭配async异步方法使用。



<!--more-->
<hr />

{% label primary@VUE %} {% label info@ES6 %}


#### 例子

> async

```
async function demo() {
　　return 'hello world';
}
console.log(demo())
console.log('run...')

运行结果：
run...
hello world
```
async函数返回的是一个promise对象，对返回值可以进行then...catch操作。

> await

```
async function demo2() {
　　 const res = await demo1()
    console.log('inner run....')
}
console.log(demo2())
console.log('run...')


运行结果：
run...
hello world
inner run....
```
await只能在async方面的里面使用，让后面的执行语句或方法要等待当前await方法的结果后才能再执行。
