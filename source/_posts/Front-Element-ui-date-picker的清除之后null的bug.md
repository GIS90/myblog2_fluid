---
title: Element-ui框架el-date-picker的清除之后NULL的问题
index_img: /img_index/index/20230312-001.png
categories:
  - [前端开发]
tags: [VUE, Element-UI]
abbrlink: 63642
date: 2023-03-12 22:23:27
updated: 2023-03-12 22:23:27
desc: Element-ui框架date-picker的清除之后NULL的问题
keywords: Element-ui, date-picker
---



#### 问题描述

在写一个高级筛选的VUE代码，使用到了***el-date-picker***日期选择器组件，设置了daterange类型，并且设置了可以清除的功能。但是使用清除之后，在提交高级筛选的查询，发现报错了，查了好久的问题所在，记录一下。

{% label info@VUE %} {% label primary@Element-UI %}

<!--more-->
<hr />

下面是VUE开发的高级筛选，***el-date-picker***日期选择器组件使用清除功能之后，组件变量会设置NULL。
![](xuanze.pic.jpg)

#### 报错

报错的截图。

![](error.pic.jpg)

```
vue.runtime.esm.js?2b0e:619 [Vue warn]: Error in v-on handler: "TypeError: Cannot read properties of null (reading '0')"

found in

---> <ElButton> at packages/button/src/button.vue
       <ElRow>
         <ElForm> at packages/form/src/form.vue
           <ApiFilter> at src/services/info/ApiFilter.vue
             <Api> at src/views/info/api.vue
               <AppMain> at src/layout/components/AppMain.vue
                 <Layout> at src/layout/index.vue
                   <App> at src/App.vue
                     <Root>
warn @ vue.runtime.esm.js?2b0e:619
logError @ vue.runtime.esm.js?2b0e:1884
globalHandleError @ vue.runtime.esm.js?2b0e:1879
handleError @ vue.runtime.esm.js?2b0e:1839
invokeWithErrorHandling @ vue.runtime.esm.js?2b0e:1862
invoker @ vue.runtime.esm.js?2b0e:2179
invokeWithErrorHandling @ vue.runtime.esm.js?2b0e:1854
Vue.$emit @ vue.runtime.esm.js?2b0e:3882
handleClick @ element-ui.common.js?5c96:9417
invokeWithErrorHandling @ vue.runtime.esm.js?2b0e:1854
invoker @ vue.runtime.esm.js?2b0e:2179
original._wrapper @ vue.runtime.esm.js?2b0e:6911
vue.runtime.esm.js?2b0e:1888 TypeError: Cannot read properties of null (reading '0')
    at VueComponent.filterQuery (ApiFilter.vue?9380:279:1)
    at click (ApiFilter.vue?f585:309:1)
    at invokeWithErrorHandling (vue.runtime.esm.js?2b0e:1854:1)
    at VueComponent.invoker (vue.runtime.esm.js?2b0e:2179:1)
    at invokeWithErrorHandling (vue.runtime.esm.js?2b0e:1854:1)
    at Vue.$emit (vue.runtime.esm.js?2b0e:3882:1)
    at VueComponent.handleClick (element-ui.common.js?5c96:9417:1)
    at invokeWithErrorHandling (vue.runtime.esm.js?2b0e:1854:1)
    at HTMLButtonElement.invoker (vue.runtime.esm.js?2b0e:2179:1)
    at original._wrapper (vue.runtime.esm.js?2b0e:6911:1)
```
#### 原因

直接说问题所在，使用***el-date-picker***日期选择器组件的清除功能，会把组件的变量设置NULL，所以导致在获取时间的时候报错。


#### 解决方案

![](i.jpg)
```
if (this.create_date !== null && this.create_date !== '' && this.create_date !== undefined) {
  this.searchData.create_time_start = this.create_date[0] + ' 00:00:00'
  this.searchData.create_time_end = this.create_date[1] + ' 23:59:59'
} else {
  this.searchData.create_time_start = ''
  this.searchData.create_time_end = ''
}
```
