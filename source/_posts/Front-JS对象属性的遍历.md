---
title: Javascript对象属性的遍历
index_img: /img_index/index/20220109-001.jpeg
categories:
  - 前端开发
tags:
  - Javascript
desc: 总结一下jquery对象遍历属性的方法
keywords: 'jquery, 数组, 对象, 遍历'
abbrlink: 27964
date: 2022-01-09 15:20:30
updated: 2022-01-09 15:20:30
---



{% label info@7种Javascript对象属性遍历的方法 %}

<!--more-->
<hr />

> 测试数据

```
var name = 'mingliang.gao';
var demo = {
    name,
    language: 'js',
    say() {
        return ('name: ', name, 'language: ', language)
    }
}
```

#### 方法一：for...in

for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。
```
for (let k in demo){
    console.log(k)
}
```
结果：
```
name
language
say
```

#### 方法二：Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。
```
console.log(Object.keys(demo))
```
结果：
```
[ 'name', 'language', 'say' ]
```

#### 方法三：Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
```
console.log(Object.getOwnPropertyNames(demo))
```
结果：
```
[ 'name', 'language', 'say' ]
```

#### 方法四：Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。
```
console.log(Object.getOwnPropertySymbols(demo))
```
结果：
```
[ 'name', 'language', 'say' ]
```

#### 方法五：Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
```
console.log(Reflect.ownKeys(demo))
```
结果：
```
[ 'name', 'language', 'say' ]
```

#### 方法六：getOwnPropertyDescriptor(obj, '属性名')

对象的每个属性都有一个描述对象（Descriptor），Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。
```
console.log(getOwnPropertyDescriptor(demo, 'name'))
```
结果：
```
{ value: 'mingliang.gao',
  writable: true,
  enumerable: true,
  configurable: true }
```

#### 方法七：解构赋值

```
let { name, language, say, ...args } = demo;
console.log(language)
console.log(say)
console.log(args)
```
结果：
```
mingliang.gao
python
```
