---
title: Python爬虫利器之BS4(下篇)
index_img: /img_index/index/article_python_crawler.jpg
categories:
  - [Python]
tags: [Python, Python包, 爬虫系列]
abbrlink: 34936
date: 2019-05-13 21:01:43
updated: 2019-05-13 21:01:43
desc: Python爬虫利器之BeautifulSoup介绍
keywords: Python, Packages, 爬虫, BeautifulSoup, requests
---

{% label info@Python包 %} {% label warning@BeautifulSoup %} {% label danger@爬虫教程系列 %}

### 1、背景

{% note primary %}

关于爬虫的教程数据获取在上篇requestts文章中已经讲解过了，本篇讲解爬虫的数据清洗利器***BeautifulSoup***，继续为<font color='red' size=6.5>爬虫</font>而服务。
{% endnote %}

<!--more-->
<hr />

### 2、简介

Pypi官方的简介：
```
Beautiful Soup is a library that makes it easy to scrape information from web pages.
It sits atop an HTML or XML parser, providing Pythonic idioms for iterating, searching,
and modifying the parse tree.
```
简明扼要，***BeautifulSoup***是用来解析HTML的工具。

### 3、安装

```
pip install beautifulsoup4
```
很多单，不多说。

### 4、解析器

#### 4.1、安装

```
pip install lxml
pip install html5lib
```

#### 4.2、比对

    {% raw %}
    <table>
      <tr>
        <th style="text-align:center;">解析器</th>
        <th style="text-align:center;">使用方法</th>
        <th style="text-align:center;">优势</th>
        <th style="text-align:center;">劣势</th>
      </tr>
      <tr>
        <td style="text-align:center;">Python标准库</td>
        <td style="text-align:center;">BeautifulSoup(markup, "html.parser")</td>
        <td style="text-align:center;">
        Python的内置标准库
        执行速度适中
        文档容错能力强
        </td>
        <td style="text-align:center;">Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差</td>
      </tr>
      <tr>
        <td style="text-align:center;">lxml HTML 解析器</td>
        <td style="text-align:center;">BeautifulSoup(markup, "lxml")</td>
        <td style="text-align:center;">
        速度快
        文档容错能力强
        </td>
        <td style="text-align:center;">需要安装C语言库</td>
      </tr>
      <tr>
        <td style="text-align:center;">lxml XML 解析器</td>
        <td style="text-align:center;">
        BeautifulSoup(markup, ["lxml-xml"])
        BeautifulSoup(markup, "xml")
        </td>
        <td style="text-align:center;">
        速度快
        唯一支持XML的解析器
        </td>
        <td style="text-align:center;">需要安装C语言库</td>
      </tr>
      <tr>
        <td style="text-align:center;">html5lib</td>
        <td style="text-align:center;">BeautifulSoup(markup, "html5lib")</td>
        <td style="text-align:center;">最好的容错性
        以浏览器的方式解析文档
        生成HTML5格式的文档
        </td>
        <td style="text-align:center;">
        速度慢
        不依赖外部扩展
        </td>
      </tr>
    </table>
    {% endraw %}

官方是推荐使用lxml作为解析器，效率更高以及容错能力也比较高，在Python2.7.3之前的版本和Python3中3.2.2之前的版本，必须安装lxml或html5lib, 因为那些Python版本的标准库中内置的HTML解析方法不够稳定.

个人也推荐使用lxml，确实比其他的解析起来快一些，如果不想安装解析器的话那么就推荐使用html5lib，因为2.7版本之后不用安装。

{% note danger %}
    <font color='red' size=5>提示</font>
    如果一段HTML或XML文档格式不正确的话,那么在不同的解析器中返回的结果可能是不一样的,查看 解析器之间的区别 了解更多细节
{% endnote %}

### 5、基础语法

```
from bs4 import BeautifulSoup


html_doc = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="title"><b>The Dormouse's story</b></p>
        <p class="story">Once upon a time there were three little sisters; and their names were
        <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
        <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
        <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
        and they lived at the bottom of a well.</p>
        <p class="story">...</p>
    </body>
</html>
"""

soup = BeautifulSoup(html_doc, "lxml")
print soup.prettify()
```
这里的html的demo直接引用与官网，基础语法主要做了2件事情，第一把html加载进来，第二设定一下解析器。
关于加载html文档这里直接是引用的string，可以用上篇中requests包，获取返回的html数据运用response的text或者content都可以；或者直接用open打开下载好的html也可以。

### 6、soup的对象说明

BeautifulSoup将HTML文档转换成一个复杂的树形结构，每个节点都是Python对象，所有对象可以归纳为4种: **Tag、NavigableString、BeautifulSoup、Comment**。

#### 6.1、Tag

***Tag对象***：对应着XML或HTML原生文档中的tag，说白了就是标签，如果连标签都不明白，我真的没法解释了。
```
print soup.p
# <p class="title"><b>The Dormouse's story</b></p>

print soup.title
# <title>The Dormouse's story</title>
```

每个Tag有两个重要的属性name和attrs。

> Name

通过.name获取tag标签:
```
p = soup.p
print p.name
# p
```

> Attributes

一个tag可能会有多个属性，.tag的属性的获取方法与dict字典相同：
```
p = soup.p
# 获取
print p.attrs
# {'class': ['title']}
# 设定set
p['class'] = 'new_title'
p['id'] = 1
print p.attrs
# {'class': 'new_title', 'id': 1}
```

> 多值属性

在HTML5中会常见的多值的属性，例如class='1 2 3'，在Beautiful Soup中多值属性的返回类型是list:
```
css_soup = BeautifulSoup('<p class="body strikeout"></p>')
css_soup.p['class']
# ["body", "strikeout"]

css_soup = BeautifulSoup('<p class="body"></p>')
css_soup.p['class']
# ["body"]
```

#### 6.2、NavigableString

***NavigableString对象***：获取标签内部的内容，这个就比较简单了，通过.string进行获取，直接看demo：
```
print p.string
# The Dormouse's story
```

#### 6.3、BeautifulSoup

***BeautifulSoup对象***：表示一个文档的全部内容，它支持遍历、搜索，没有name和attribute属性。
说白了，这个其实就把html作为一个soup对象了，在基础方法中的设定。

#### 6.4、 Comment

***Comment对象***：这个对象是用来解决html文档中的注释用的，它是一个特殊类型的NavigableString对象。
在这里，直接引用了官方的demo：
```
markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
soup = BeautifulSoup(markup)
comment = soup.b.string
print comment
# Hey, buddy. Want to buy a used parser?
print type(comment)
# <class 'bs4.element.Comment'>
```

### 7、遍历

关于遍历不进行详细讲解了，主要没咋用过，做爬虫就是数据获取、清洗，至于遍历数据，还不如直接进行搜索，感兴趣的可以在**学习参考**中去官方进行学习。
在这里列举一下我用过并且觉得常用的方法：

#### 7.1、子节点

通过.contents 和 .children，可以将tag的子节点以列表的方式输出。
```
head_tag = soup.head
print head_tag
# <head><title>The Dormouse's story</title></head>

print head_tag.contents
# [<title>The Dormouse's story</title>]
print len(head_tag.contents)
# 1

for child in title_tag.children:
    print child
    # <title>The Dormouse's story</title>
```

#### 7.2、父节点

通过.parent属性来获取元素的父节点，只获取当前元素的父节点而且只寻找一次。
```
title_tag = soup.title
print title_tag
# <title>The Dormouse's story</title>
print title_tag.parent
# <head><title>The Dormouse's story</title></head>
```
通过.parents属性可以递归得到元素的所有父辈节点，一级一级的向父节点去寻找，最终寻找到根节点。
```
link = soup.a
print line
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
for parent in link.parents:
    if parent is None:
        print parent
    else:
        print parent.name
# p body html [document] None
```

### 8、搜索

这个是重点，因为数据索取主要通过搜索来进行获取，BeautifulSoup官方文档主要介绍了find()和find_all()搜索方法，但是根据传入的参数不一样，方法又会变成多种，这里列举一些常用的搜索方法。
先简单说一下find()和find_all()方法的区别：1.就是find()返回搜索查询到的第一条数据，find_all()会返回所有查询到的数据；2.find_all()返回的数据类型是**class 'bs4.element.ResultSet**，find()返回的数据类型**class 'bs4.element.Tag**，可知find_all()返回的是一个set集合类型。
日常搜索数据中还是用find_all()比较多，下面的方法介绍我也使用find_all()进行举例。

源码方法：find_all( name , attrs , recursive , string , **kwargs )

#### 8.1、标签名

根据tag也就是标签名字进行搜索。
```
a = soup.find_all('a')
print a
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```
也可以根据多个标签名进行查看，具体看demo：
```
ab = soup.find_all(["a", "b"])
print ab
# [<b>The Dormouse's story</b>,
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

#### 8.2、attr属性

可以根据标签的内在属性进行查询，参数可以写多个，下面例子给出了多种写法。
```
link1_1 = soup.find_all(attrs={'id': 'link1', 'key': 'value'})
link1_2 = soup.find_all(id='link1')
print link1_1, link1_2
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
```
如果在查询数据中，属性固定可以多用此方法。

#### 8.3、class

通过**class_**参数搜索有指定CSS类名的tag，class在Python中是保留字，所以使用***class_***，使用看demo:
```
sister_css = soup.find_all(class_="sister")
print sister_css
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

#### 8.4、string

通过string参数可以搜搜文档中的字符串内容,与name参数的可选值一样，string参数接受字符串、正则表达式、列表、True。
```
print soup.find_all(string="Elsie")
# [u'Elsie']

print soup.find_all(string=["Tillie", "Elsie", "Lacie"])
# [u'Elsie', u'Lacie', u'Tillie']

print soup.find_all(string=re.compile("Dormouse"))
# [u"The Dormouse's story", u"The Dormouse's story"]
```

#### 8.5、limit

这个参数的使用，说白了与SQL的limit用法一样，限制返回数据的条数，防止文档树很大导致搜索响应过慢。
```
print soup.find_all("a", limit=2)
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```

#### 8.6、 recursive

设置是否进行遍历子节点，调用tag的find_all()方法时，BeautifulSoup会检索当前tag的所有子孙节点，如果只想搜索tag的直接子节点，可以使用参数recursive=False。

```
print soup.html.find_all("title")
# [<title>The Dormouse's story</title>]

print soup.html.find_all("title", recursive=False)
# []
```

#### 8.7、 get_text() 方法

获取到tag中包含的所有文本内容。
```
print soup.find_all('a', string='Elsie')[0].get_text()
# 'Elsie'
print soup.find_all('a', string='Elsie')[0].string
# 'Elsie'
```
#### 8.8、 组合式

所有的参数可以一起使用，增加过滤条件加快搜索效率，前面涉及的1～6参数都可以进行组合，写了几个简单的小demo：
```
sister_css = soup.find_all('a', class_="sister")
sister_css = soup.find_all('a', class_="sister", limit=2)
link1 = soup.find_all('a', attrs={'id': 'link1'})
link1 = soup.find_all('a', attrs={'id': 'link1'}, recursive=False)
strings = soup.find_all('a', string=["Tillie", "Elsie", "Lacie"])
```

### 9、总结

本篇主要讲述了BeautifulSoup的基本使用方法，感兴趣的、深入研究的请自行查看官方方法，具体的地址也在下面整理出来了。

### 10、学习参考

- BeautifulSoup(4.4.0)中文文档：http://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/
- Pypi介绍：https://pypi.org/project/beautifulsoup4/
- BeautifulSoup(4.4.0)官方文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc/
