---
title: 关于VUE2的通过v-for动态变量的ref标签
index_img: /img_index/index/20230630-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 32337
date: 2023-06-30 21:15:02
updated: 2023-06-30 21:15:02
desc: 关于VUE2的通过v-for动态变量的ref标签
keywords: VUE, ref
---




{% note primary %}
最近使用到了VUE2的ref这个功能吧，其实功能就等于id，但是比id更好用，只要对标签注册了ref变量，那么在this.$refs中可用使用这个元素了，对元素可以进行相关的js操作。
{% endnote %}


<!--more-->
<hr />

#### 背景

ref不仅可以对标签注册，还可以对组件注册，自己写前端代码的时候用到了对img标签的v-for操作，每个img标签都进行ref注册，具体代码如下：
```
<viewer ref="viewer" :options="viewerOption" :images="images" @inited="viewerInited">
  <img v-for="(img, index) in images" :id="img.md5_id" :ref="img.md5_id" :key="index" :src="img.url" class="viewer-box-image" @click="selectViewerImage(index)">
</viewer>
```
其中，images是个字典数组，格式：[{id: 1, md5_id: 1, url: 1}]。
对每一个图片都有一个点击事件，触发事件代码如下：
```
selectViewerImage(selectIndex) {
  if (this.images && selectIndex >= 0) {
    // 设置当前图片选择的索引
    this.curImageIndex = selectIndex
    // 设置选择的样式
    this.images.forEach((item, index) => {
      // 清空image选择样式
      // document.getElementById(item.md5_id).classList.remove('viewer-select-image')
      this.$refs[item.md5_id][0].classList.remove('viewer-select-image') // ref动态变量[0]

      if (index === selectIndex) {
        // 当前选择image-md5
        this.curImage = item.md5_id
        // 当前被选择查看的image的样式
        // document.getElementById(item.md5_id).classList.add('viewer-select-image')
        this.$refs[item.md5_id][0].classList.add('viewer-select-image')
      }
    })
  }
}
```
方法的意思就是对所有的图片清除选择的样式，对选择的图片新增选中样式。但是当时我用this.$refs[item.md5_id]去获取对应的img元素的时候，发现元素获取不到，查阅了相关资料，说如果是v-for使用ref的动态变量，获取元素需要获取第一个值，也就是this.$refs[item.md5_id][0]，这个写法。

#### ref的v-for用法

- VUE中如果通过v-for遍历的方式注册ref的时候，如果是变量写法为：:ref = 变量，:（冒号）为代表这个为变量；如果是固定值就去掉 :号。
- ***通过v-for注册的ref，获取指定的ref时需要加 [0]，写法：this.$refs[变量][0]；如果是固定值或者非v-for遍历的方式，写法都是this.$refs[变量]，不需要添加[0]。***



<font size=6.5 color='red'>每天一点点，坚持学习。。。。。。</font>
