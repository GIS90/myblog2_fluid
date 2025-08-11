---
title: 程序猿七夕-心心相印
index_img: /img_index/index/20180813-001.gif
desc: 利用turtle python包去帮助程序猿过七夕，心心相印程序
categories:
  - [Python]
tags: [Python, Python程序篇]
keywords: python, turtle, 七夕, 脚本, 程序, 浪漫, 心心相印, 一箭穿心
abbrlink: 38642
date: 2018-08-13 14:19:57
updated: 2018-08-13 17:19:57
---

{% note primary %}
python带你过七夕，让程序猿的七夕不再无趣^_^。
{% endnote %}


{% label default@Python %} {% label primary@实例 %} {% label success@七夕程序 %} {% label danger@turtle %}

<!--more-->
<hr />

### 程序员

一提到程序猿，大家的映象是***这样***

![](chengxuyuan2.jpg)

***这样***

![](chengxuyuan3.gif)

***或者这样***

![](chengxuyuan4.jpg)

***其实，标配是这样***

![](chengxuyuan1.png)

哈哈，很搞笑是不，我们是程序猿，时代电子技术的领导者，怎么可能那么low呢。言归正传，程序猿也是懂浪漫的，马上七夕了，用一副漫画***<font color="#dd0000" size="5">心心相印</font>***程序猿专有方式来陪你们过七夕，拿走不谢。

### 代码

> 代码如下：

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------
describe:

usage:


base_info:
    __version__ = "v.10"
    __author__ = "mingliang.gao"
    __time__ = "2018/8/13"
    __mail__ = "mingliang.gao@qunar.com"
------------------------------------------------
"""

import time
import turtle
from turtle import *


def write_zi(x, y, content, font_type='Comic Sans MS', font_size=30, font_normal='normal'):
    penup()
    goto(x, y)
    pendown()
    write(content, align="left", font=(font_type, font_size, font_normal))


def pen_config(size=2, color='black', speed='slow'):
    # 画笔宽度
    turtle.pensize(size)
    # 画笔颜色
    turtle.pencolor(color)
    # 画笔速度 0-10
    turtle.speed(speed)


def locinit_config():
    # 设置框位置
    turtle.setup(width=1400, height=900, startx=0, starty=200)
    # turtle.screensize(bg='navajowhite')
    turtle.screensize(bg='papayawhip')


# 第一个心
def one_heart():
    color('deeppink', 'red')
    penup()
    goto(120, -20)
    begin_fill()
    pendown()
    left(45)
    fd(240)
    circle(100, 225)
    seth(90)
    circle(100, 225)
    fd(240)
    end_fill()


# 第二个心
def two_heart():
    color('orange', 'yellow')
    penup()
    goto(-40, -80)
    pendown()
    begin_fill()
    left(90)
    fd(240)
    circle(100, 225)
    seth(90)
    circle(100, 225)
    fd(240)
    end_fill()


# 箭
def arrow():
    # bing
    penup()
    goto(-410, 0)
    pendown()
    goto(-100, 60)
    penup()
    goto(255, 110)
    pendown()
    goto(540, 160)
    # tou
    penup()
    goto(440, 190)
    pendown()
    goto(540, 160)
    penup()
    goto(430, 100)
    pendown()
    goto(540, 160)


# 诗题
def shi_title():
    write_zi(200, -100, "爱", font_size=42)
    write_zi(260, -100, "你", font_size=42)
    write_zi(320, -100, "一", font_size=42)
    write_zi(380, -100, "生", font_size=42)
    write_zi(440, -100, "十", font_size=42)
    write_zi(500, -100, "世", font_size=42)


# 诗
def shi():
    """
    一半飘零一半安，
    生逢盛世缘相遇，
    十年修得同船渡，
    世世生生武雅楠
    """
    # 1
    write_zi(260, -160, "一")
    write_zi(260, -200, "半")
    write_zi(260, -240, "飘")
    write_zi(260, -280, "零")
    write_zi(260, -320, "一")
    write_zi(260, -360, "半")
    write_zi(260, -400, "安")
    # 2
    write_zi(330, -160, "生")
    write_zi(330, -200, "逢")
    write_zi(330, -240, "盛")
    write_zi(330, -280, "世")
    write_zi(330, -320, "缘")
    write_zi(330, -360, "相")
    write_zi(330, -400, "遇")
    # 3470
    write_zi(400, -160, "十")
    write_zi(400, -200, "年")
    write_zi(400, -240, "修")
    write_zi(400, -280, "得")
    write_zi(400, -320, "同")
    write_zi(400, -360, "船")
    write_zi(400, -400, "渡")
    # 4
    write_zi(470, -160, "世")
    write_zi(470, -200, "世")
    write_zi(470, -240, "生")
    write_zi(470, -280, "生")
    write_zi(470, -320, "武")
    write_zi(470, -360, "雅")
    write_zi(470, -400, "楠")


if __name__ == '__main__':
    time.sleep(2)
    locinit_config()
    pen_config(size=8, speed='fast')
    one_heart()
    pen_config(size=8, speed='fast')
    two_heart()
    pen_config(size=12, color='black', speed='normal')
    arrow()
    pen_config(size=3, color='red', speed='normal')
    shi_title()
    pen_config(size=3, color='dodgerblue', speed='fast')
    shi()
    time.sleep(5)
```

> ***备注***

- 绘画顺序：心->心->箭->诗
- pen_config 画笔设置，在这个方法可以切换绘画过程中的画笔颜色以及绘画的速度
- shi 为七言绝句，如果想改其他，记得调整位置

### 实现效果

![](/img_index/index/20180813-001.gif)