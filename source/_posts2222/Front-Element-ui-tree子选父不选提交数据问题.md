---
title: ElementUI的tree组件子选父不选提交数据问题
index_img: /img_index/index/20220515-001.png
categories:
  - [前端开发]
tags: [VUE, Element-UI]
top: false
abbrlink: 35413
date: 2022-05-15 08:51:55
updated: 2022-05-15 08:51:55
desc: Element-UI的tree组件子选父不选提交数据问题
keywords: VUE, Element-UI
---


#### 问题描述

最近也写了一段时间的VUE了，用的就是Element-UI组件，今天使用tree组件的时候，发现***el-tree***控件中子节点未全部选中，提交树形数据时，后台API接收数据时没有发现父节点，完了在前端console打印了一下log，发现***el-tree***组件没有提交处于半选择状态的节点ID，记录一下解决方案。

{% label info@VUE %} {% label primary@Element-UI %}

<!--more-->
<hr />

#### 原因

Element-UI组件中el-tree控件中子节点未全部选中时，父节点ID在提交时不会传给后台接口，导致后台获取不到父节点ID，所以后台API数据中没有半选择的父节点ID数据。

#### 解决方案

直接上代码：
```
const checked = this.$refs.menuTree.getCheckedKeys() // 全选状态ID
const halfChecked = this.$refs.menuTree.getHalfCheckedKeys() // 半选状态ID

// 合并数据方式一
const keys = [...checked, ...halfChecked]
// 合并数据方式二
const keys = checked.concat(halfChecked)
```
提交的部分就自己写了，主要就是封装axios成功方法，Promise去获取数据。
