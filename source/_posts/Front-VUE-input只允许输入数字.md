---
title: VUE之input只允许输入数字
index_img: /img_index/index/20230106-001.png
categories:
  - [前端开发]
tags: [VUE]
abbrlink: 18893
date: 2023-01-06 21:48:38
updated: 2023-01-06 21:48:38
desc: VUE之input只允许输入数字
keywords: VUE, input
---



{% note primary %}
加工INPUT标签，只允许输入数字
{% endnote %}

{% label primary@VUE %} {% label info@INPUT %}

<!--more-->
<hr />

在写前端代码的时候，经常会遇到手机号的input输入框，除了位数限制，还有数字等判断，对input进行加工，满足纯数字输入：

#### 方式一
直接用oninput事件处理输入的内容，正则匹配内容
```
<el-input
    v-model.trim="formData.start"
    placeholder="文档转换的起始页码，默认首页开始"
    :size="inputAttrs.size"
    :maxlength="formDataLimit.start"
    :clearable="inputAttrs.clear"
    :show-word-limit="inputAttrs.limit"
    :prefix-icon="inputAttrs.prefixIcon"
    :disabled="!formData.mode"
    oninput="this.value=this.value.replace(/[^\d]/g,'');"
/>
```

#### 方式二
直接使用Element-UI的el-input-number组件。
```
<el-input-number v-model="num" @change="handleChange" :min="1" :max="10" label="描述文字"></el-input-number>
```




{% raw %}
<div class="post_cus_note">END</div>
{% endraw %}
