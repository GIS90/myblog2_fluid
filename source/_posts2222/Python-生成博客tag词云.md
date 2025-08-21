---
title: Python生成博客tags词云
index_img: /img_index/index/20181121-001.png
categories:
  - [Python]
tags: [Python, Python程序篇, Hexo]
abbrlink: 31753
date: 2018-11-21 15:39:04
updated: 2018-11-21 15:39:04
desc: 利用Python写的一个生成博客tag词云脚本，用于hexo博客关于我中博客技术统计使用
keywords: Python, hexo, 词云, tag, 脚本, 程序
---

{% label default@Python %} {% label primary@Tag词云 %} {% label success@Python实例 %} {% label info@Hexo %}

#### 背景
{% note primary %}
博客已经了一些技术文章，在写about页面的时候，想把自己tags分类展示在页面上，但是直接写文字，能不能在low点。。。想到了词云，于是写个脚本自动生产博客的词云图片，<font color='red' size=4.5>完美</font>！！！。
{% endnote %}



<!--more-->
<hr />

#### 环境

| id  |  name  | Version |
|:---:|:------:|:-------:|
|  1  | Python |   2.7   |

#### 数据流

<font color='red' size=5>html数据源 -> html解析收集tag -> 词云生成图片</font>

#### 正文

脚本是针对博客建立的，所以数据源取的就是博客文章的tag标签。脚本开发起来很简单，代码也不是很复杂，每个方法有基本的注释，代码如下：
```
# -*- coding: utf-8 -*-

"""
------------------------------------------------
describe:
    词云生成器

usage:
    python tag_cloud.py

base_info:
    __version__ = "v.10"
    __author__ = "mingliang.gao"
    __time__ = "2018/11/21"
    __mail__ = "mingliang.gao@qunar.com"
------------------------------------------------
"""

import os
import re
import jieba
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from wordcloud import WordCloud
from bs4 import BeautifulSoup


BACKGROUND_NAME = 'public/images/tags_background.png'
HTML_REL_DIR = 'public/articles'
TAR_IMG = 'public/images/blog_tags.jpg'


def get_cur_dir():
    return os.path.abspath(os.path.dirname(__file__))


class TagCloudGenerator(object):
    def __init__(self):
        self.background = os.path.join(get_cur_dir(), BACKGROUND_NAME)
        self.text_source = os.path.join(get_cur_dir(), HTML_REL_DIR)
        self.tar_img = os.path.join(get_cur_dir(), TAR_IMG)

    def get_index_files(self):
        """
        get all index.html file
        :return: list type
        """
        index_files = list()
        pattern = re.compile('index\.html')
        for root, dirs, files in os.walk(self.text_source):
            for f in files:
                f_full = os.path.join(root, f)
                match = pattern.search(f_full)
                index_files.append(f_full) if match else None
        else:
            return index_files

    def get_index_tags(self, index_html):
        """
        get tags of index.html
        :param index_html: index html
        :return: list type
        """
        tags = list()
        if not index_html:
            return tags
        if not os.path.exists(index_html):
            return tags

        def _deal_html_tas():
            soup = BeautifulSoup(open(index_html), 'html.parser', from_encoding='utf-8')
            post_a_tags = soup.find_all('a', attrs={"rel": "tag"})
            if post_a_tags:
                for tag in post_a_tags:
                    tag_text = tag.get_text()
                    tags.append(tag_text.strip())
            return tags

        return _deal_html_tas()

    def collect_tags(self):
        all_tags = list()
        all_indexs = self.get_index_files()
        for index_file in all_indexs:
            if not index_file:
                continue

            tags = self.get_index_tags(index_file)
            print index_file
            all_tags.extend(tags) if tags else None
        else:
            return all_tags

    def run(self):
        all_html_tags = self.collect_tags() * 100
        wl_space_split = " ".join(all_html_tags)
        d = os.path.dirname(__file__)
        tag_background = np.array(Image.open(self.background))
        my_wordcloud = WordCloud(background_color="#CACFD2",
                                 max_words=2000,
                                 font_path="public/publicfiles/fangsong_GB2312.ttf",
                                 mask=tag_background,
                                 stopwords={'企业'},
                                 max_font_size=150,
                                 scale=1,
                                 width=800,
                                 random_state=1).generate(wl_space_split)

        plt.imshow(my_wordcloud)
        plt.axis("off")
        # plt.show()
        my_wordcloud.to_file(self.tar_img)


if __name__ == '__main__':
    tc = TagCloudGenerator()
    tc.run()
```
代码存在tag_cloud.py文件中，放在blog的根目录，图片生成的位置：blog/public/images/blog_tags.jpg。

#### 执行方法
```
python tag_cloud.py
```

#### 嵌入about

找到about页面的index.md文件，嵌入图片，代码如下：
```
<img src="/images/blog_tags.jpg"  alt="tags cloud" width="88%"/>
```
