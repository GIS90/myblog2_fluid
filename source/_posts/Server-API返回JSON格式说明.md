---
title: Server后端API返回JSON格式说明
index_img: /img_index/index/20211102-001.png
categories:
  - [服务器]
tags: [API, JSON]
top: false
abbrlink: 40461
date: 2021-11-02 23:08:09
updated: 2021-11-02 23:08:09
desc: 总结一下后台API数据交换的格式
keywords: API, JSON, 数据, 交换
---

开发中经常与前端开发或者自己写的前端项目进行数据交换，总结一下自己常用的数据交换格式，以及状态码对应的内容。

{% label danger@Linux %} {% label info@Python %}

<!--more-->
<hr />

> 格式

***JSON****

```
{
    "status_id": 100,
    "status": "成功",
    "message": "请求内容",
    "data": [],
}
```

- status_id：状态码
- status：状态内容
- message：错误信息
- data：请求的json数据内容

> 实例

| Status_id | Status  | Msg                              | Data |
|:---------:|:------- |:-------------------------------- |:---- |
|    100    | success | 请求成功                         | {}   |
|    101    | success | 请求成功，查询数据为空           | {}   |
|    201    | failure | 请求方法错误                     | {}   |
|    202    | failure | 缺少请求参数                     | {}   |
|    203    | failure | 请求参数不合法                   | {}   |
|    204    | failure | 请求参数为必须信息               | {}   |
|    205    | failure | 缺少rtx_id信息                   | {}   |
|    206    | failure | 缺少上传文件                     | {}   |
|    207    | failure | 文件格式不支持                   | {}   |
|    208    | failure | 文件内容不符合要求               | {}   |
|    209    | failure | 文件行内容有问题                 | {}   |
|    210    | failure | 文件存储失败                     | {}   |
|    211    | failure | 参数特殊要求                     | {}   |
|    212    | failure | 文件数据已存在                   | {}   |
|    213    | failure | 文件导出数据为空                 | {}   |
|    301    | failure | 数据已存在，无需重新建立         | {}   |
|    302    | failure | 数据不存在                       | {}   |
|    303    | failure | 部分数据处理成功                 | {}   |
|    304    | failure | 数据已删除，无需处理             | {}   |
|    305    | failure | 数据已处理，无须二次处理         | {}   |
|    306    | failure | 数据已删除，无须二次删除         | {}   |
|    307    | failure | 数据处理失败                     | {}   |
|    308    | failure | 数据不可用                       | {}   |
|   3081    | failure | 数据（地址）不可用               | {}   |
|   3082    | failure | 数据（姓名）不可用               | {}   |
|   3083    | failure | 数据（重量）不可用               | {}   |
|   3084    | failure | 数据（电话）不可用               | {}   |
|    309    | failure | 数据创建用户与当前更改用户不一致 | {}   |
|    310    | failure | 非数据相关人员，禁止更新数据     | {}   |
|    401    | failure | 图片格式不支持                   | {}   |
|    402    | failure | 图片存储失败                     | {}   |
|    403    | failure | 文章创建者与提交者不符合         | {}   |
|    450    | failure | 数据存储失败                     | {}   |
|    501    | failure | Server发生错误，获取数据失败     | {}   |
|    601    | failure | HTTP请求接口失败                 | {}   |
|    602    | failure | HTTP请求数据为空                 | {}   |
|    603    | failure | HTTP请求数据不可用               | {}   |
