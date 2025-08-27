---
title: Python处理ArcGIS多图层矢量裁剪
index_img: /img_index/index/20151012-001.png
categories:
  - [工具集]
  - [Python]
tags: [Python, ArcGIS, Python程序篇]
keywords: python, ArcGIS, 脚本, 程序
desc: python编写的脚本自动化处理ArcGIS多图层矢量裁剪
abbrlink: 9995
date: 2015-10-12 15:55:30
---

基于***Hexo***命令自建博客的开篇文章，写一篇关于第一份工作的专业内容，结合Python自动处理矢量数据，大大的提升工作效率。

<!--more-->
<hr />

### 1、序言

{% note primary %}
分享一下利用python调用ArcGIS函数实现矢量多图层裁剪，出现问题请发邮箱留言，帮你解决，不过我都调试好了，问题不大。python的使用暂时先不做分享了(主要我也在自学阶段)，代码很少但是很实用，所以把自己的东西跟大家***share***一下，以后会陆续跟大家一起分享交流，希望对大家有帮助。
{% endnote %}

### 2、环境相关

开发语言：python2
系统：win7

### 3、相关代码

```python
    # -*- coding: utf-8 -*-
    
    # 导入包
    import arcpy
    import os
    import datetime


    startTime=datetime.datetime.now()
    print "python tool start--------^_^---------"
    
    # 裁剪文件的工作空间
    InputSpace=r"F:\data\fuzhou_data"
    # 结果文件的存放目录
    OutputSpace=r"F:\data\fuzhou_map_Demo"
    # 被裁剪文件路径+名称
    clip_features=r"F:\data\clip\clip.shp"
    
    # 实现的主体，添加个变量用于处理次数
    num=1
    # 设置工作空间
    arcpy.env.workspace=InputSpace
    for in_features in arcpy.ListFiles("*.shp"):
        clipName=os.path.splitext(in_features)[0]
        out_features=os.path.join(OutputSpace,clipName)
        cluster_tolerance="0.0000001 DecimalDegrees"
        print "Execute num=", num, "Chip feature is:", clipName
    
        try:
            arcpy.Clip_analysis(in_features, clip_features,
            out_features, cluster_tolerance)
            print "Finish"
            num=num+1
        except Exception as e:
            print e.message
    
    endTime=datetime.datetime.now()
    exeTime=(endTime-startTime).seconds
    print "sum=", num, "All finish, cost time is :", exeTime,"s"
```

### 4、结束语

<span style="font-size: 25px;color: red;">***坚持学习，希望可以在喜欢的道路上越走越远\~\~\~***</span>

