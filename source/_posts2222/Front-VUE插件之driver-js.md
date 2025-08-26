---
title: VUE插件之driver.js
index_img: /img_index/index/20220424-001.jpg
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 29277
date: 2022-04-24 10:03:11
updated: 2022-04-24 10:03:11
desc: VUE插件之driver.js
keywords: VUE, 插件, driver, 向导
---

{% note success %}
在开发OpenTool开源工具的时候，在个人中心部分有一个向导，上网查了一下用driver.js，记录一下各项用到的内容，挺好用的VUE插件，推荐。
{% endnote %}


{% label primary@VUE %} {% label info@插件 %}

<!--more-->
<hr />

#### 1、作用

用于开发Web页面功能操作向导，官网介绍的很明确，而且也没啥难点，直接贴上代码了。

#### 2、安装

```
npm install -S driver.js
```

#### 3、VUE代码

```
<template>
  <div class="app-container">
    <div id="guide-opr-container">
      <el-button icon="el-icon-position" type="primary" @click.prevent.stop="guide">
        开启向导
      </el-button>
    </div>
    <div id="guide-data-container" class="guide-data-container">
      <aside>
        The guide page is useful for some people who entered the project for the first time. You can briefly introduce the
        features of the project
      </aside>
    </div>
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import steps from './guide/steps.js'

export default {
  name: 'Guide',
  data() {
    return {
      driver: null
    }
  },
  mounted() {
    // parameter config
    // 官网API：https://www.npmjs.com/package/driver.js
    this.driver = new Driver({
        className: 'scoped-class', // className to wrap driver.js popover
        animate: true, // 是否设置动画
        opacity: 0.7, // 背景不透明度（0表示只有弹出框，没有覆盖）
        padding: 0, // 元素与边缘之间的距离
        allowClose: true, // 单击覆盖是否应关闭
        overlayClickNext: false, // 点击覆盖是否应该下一步移动
        stageBackground: '#ffffff', // default: #ffffff，突出显示元素的背景色
        showButtons: true, // 页脚中是否显示控制按钮
        keyboardControl: true, // 允许通过键盘进行控制（退出关闭，箭头键移动）
        doneBtnText: '完成', // 完成按钮文本
        closeBtnText: '关闭', // 关闭按钮文本
        prevBtnText: '上一步', // 上一步按钮文本
        nextBtnText: '下一步', // 下一步按钮文本
        onHighlightStarted: (Element) => {}, // 当元素即将高亮显示时调用
        onHighlighted: (Element) => {}, // 当元素完全高亮显示时调用
        onDeselected: (Element) => { // 取消选择元素时调用
          this.onDeselected(Element)
        },
        onReset: (Element) => {}, // 将要清除覆盖时调用
        onNext: (Element) => {}, // 在任何步骤上移动到下一步时调用
        onPrevious: (Element) => {} // 在任何步骤上移动到上一步时调用
    })
  },
  methods: {
    guide() {
      this.driver.defineSteps(steps)
      this.driver.start()
    }
  }
}
</script>

<style scoped>
.guide-data-container {
  margin-top: 20px;
}
</style>
```

#### 4、Steps代码

需要在同等目录下建立一个guide文件夹，再建立一个steps.js文件，用来存放向导的数据，根据不同的模块定制不同的js文件。
```
const steps = [
  {
    element: '#sidebar-container',
    popover: {
      title: '系统菜单',
      description: '系统功能菜单列表，分为一级菜单、二级菜单',
      position: 'right', // position: left, left-center, left-bottom, top,
      // top-center, top-right, right, right-center, right-bottom,
      // bottom, bottom-center, bottom-right, mid-center
      offset: 20
    },
    padding: 0
  },
  {
    element: '#guide-opr-container',
    popover: {
      title: '导航菜单',
      description: '打开&&关闭导航菜单',
      position: 'bottom', // position: left, left-center, left-bottom, top,
      // top-center, top-right, right, right-center, right-bottom,
      // bottom, bottom-center, bottom-right, mid-center
      offset: 20
    },
    padding: 0
  },
  {
    element: '#guide-data-container',
    popover: {
      title: '数据域',
      description: '系统定制功能的数据展示域',
      position: 'bottom',
      offset: 0
    },
    padding: 0
  }
]

export default steps
```

#### 5、学习参考

driver.js官网：https://kamranahmed.info/driver.js/#single-element-no-popover
npm查询：https://www.npmjs.com/package/driver.js
