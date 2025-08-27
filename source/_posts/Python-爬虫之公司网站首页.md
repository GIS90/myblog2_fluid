---
title: Python爬虫之公司网站首页Title、Keywords、Description
index_img: /img_index/index/article_python_crawler.jpg
categories:
  - [Python]
tags: [Python, 爬虫系列, Python程序篇]
abbrlink: 43681
date: 2019-11-04 16:38:46
updated: 2019-11-05 16:38:46
desc: 利用python爬虫技术获取指定公司网站首页的Title、Keywords、Description
keywords: python, 爬虫, requests, BeautifulSoup, bs4, Title, Keywords, Description, 脚本, 程序
---

{% note primary %}
工作上涉及公司网站SEO优化相关的工作，但是在<font color='red' size=6.5>Title、Keywords、Description</font>这块有点不清楚。于是，参考了一些与自己公司业务上有相同的公司网站内容，在进行词语分析选取一些关键词，在结合本公司的业务内容关键词进行综合，完成了网站SEO优化的<font color='red' size=6.5>Keywords</font>。
{% endnote %}




<!--more-->


{% label default@Python实战 %} {% label primary@爬虫教程系列 %}

<hr />

废话也不多说了，关于爬虫相关使用的教程在前面以及介绍了，而且还有栗子，这里主要贴上相关代码。除了爬取数据之外，本程序还把处理好的数据进行分析选取出现频率最多***Keywords***，以及把爬取的数据存储到excel中。

<img src="article_kwpachong.jpg" style="border:1.5px solid blue"/>

### 思路

抓取数据 -> 解析 -> 存储 -> 分析

### 特色

在本次爬虫中，运用了***gevent协程***，玩python不清楚协程的同学请自行脑补。

### 代码

{% code %}
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    用来抓取指定的物流公司官网的信息，包含title、keywords、description。

usage:
    python comp_infos_grab.py


base_info:
    __version__ = "v.10"
    __author__ = "PyGo"
    __time__ = "2019/12/3"
    __mail__ = "gaoming971366@163.com"

------------------------------------------------
"""
import requests
import gevent
import xlrd
import xlwt
from gevent import monkey; monkey.patch_all()
from bs4 import BeautifulSoup
import jieba

PUBLIC_URL_LIST = {
    "IML俄罗斯海外仓": "http://www.imlb2c.com/",
    "旺集科技": "http://www.wangjigroup.com/",
    "黑龙江俄速通国际物流有限公司": "http://www.ruston.cc/",
    "AliExpress全球速卖通": "https://sell.aliexpress.com/zh/__pc/shipping/aliexpress_shipping.htm",
    "中外运集装箱运输有限公司": "http://www.sinolines.com/",
    "乐泰国际物流有限公司": "http://www.letaimzl.com/",
    "NOEL诺艾尔集团": "http://www.noelworld.com/",
    "慧承国际物流": "http://www.hcwuliu.com/",
    "满洲里新颖国际货运代理有限公司": "http://www.mzlxinying.com/",
    "运盟国际物流": "http://www.ym-trans.com/",
    "如易科技": "http://www.ruecom.cn/"
}


class companyGrap(object):
    _instance = None

    def __init__(self):
        super(companyGrap, self).__init__()
    
    def __new__(cls, *args, **kwargs):
        if companyGrap._instance is None:
            companyGrap._instance = object.__new__(cls, *args, **kwargs)
    
        return companyGrap._instance
    
    def _get_infos(self, url):
    
        results = dict()
        results['url'] = url
    
        if not url:
            return results
    
        payload = ""
        headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36"
            }
        response = requests.get(url, data=payload, headers=headers)
        if response.status_code == 200:
            soup = BeautifulSoup(response.content, 'html.parser')
            head = soup.head
            titles = head.find_all('title')
            tl = titles[0].string if titles else ""
            results['title'] = tl
            keywords = head.find_all('meta', attrs={'name': 'keywords'})
            kw = keywords[0].attrs.get('content') if keywords else ""
            results['keyword'] = kw
            descriptions = head.find_all('meta', attrs={'name': 'description'})
            desc = descriptions[0].attrs.get('content') if descriptions else ""
            results['description'] = desc
    
        return results
    
    def to_excel(self, datas, exlname):
        """
        generate data of excel format to save
        :param datas: excel data
        :param exlname: excel name
        :return: None, excel data
        """
        f = xlwt.Workbook(encoding='utf-8')
        sheet = f.add_sheet('sheet', cell_overwrite_ok=True)
        EXCEL_TITLES = ["ID", "NAME", "URL", 'TITLE', 'KEYWORDS', 'DESCRIPTION', "REMARK"]
        BUSINESS = "BUSINESS"
    
        style_title = xlwt.XFStyle()
        font = xlwt.Font()
        font.name = 'Times New Roman'
        font.bold = True
        font.color_index = 4
        font.height = 220
        style_title.font = font
    
        style_content = xlwt.XFStyle()
        font = xlwt.Font()
        font.name = 'Times New Roman'
        font.bold = False
        font.color_index = 4
        font.height = 220
        style_content.font = font
    
        # 标题
        for i in range(0, len(EXCEL_TITLES)):
            sheet.write(0, i, EXCEL_TITLES[i], style_title)
    
        # 合并 && 重写
        sheet.write_merge(0, 0, 3, 5, BUSINESS, style_title)
        sheet.write_merge(0, 1, 0, 0, 'ID', style_title)
        sheet.write_merge(0, 1, 1, 1, 'NAME', style_title)
        sheet.write_merge(0, 1, 2, 2, 'URL', style_title)
        sheet.write_merge(0, 1, 6, 6, 'REMARK', style_title)
        for i in range(3, 6):
            sheet.write(1, i, EXCEL_TITLES[i], style_content)
    
        row = 2
        count = 1
        for line in datas:
            sheet.write(row, 0, count, style_title)
            sheet.write(row, 1, line.get('name'), style_content)
            sheet.write(row, 2, line.get('url'), style_content)
            sheet.write(row, 3, line.get('title'), style_content)
            sheet.write(row, 4, line.get('keyword'), style_content)
            sheet.write(row, 5, line.get('description'), style_content)
            row += 1
            count += 1
    
        f.save(exlname)
    
    def _deal_url(self, k, v):
        return self._get_infos(v)
    
    def to_generate_kw(self, datas):
        keywords_src = ""
        for data in datas:
            if not data:
                continue
            keywords_src += data.get('keyword')
    
        keywords = jieba.lcut(keywords_src, cut_all=False)
        counts = dict()
        for word in keywords:
            if not word:
                continue
            if isinstance(word, unicode):
                word = word.encode('utf-8')
            if word in ('|', ',', ' ', '-', '，'):
                continue
            counts[word] = counts.get(word, 0) + 1
    
        ord_counts = sorted(counts.items(), key=lambda x: x[1], reverse=True)
        for k in ord_counts:
            print "%s: %s" % (k[0], k[1])
    
    def run(self, to_excel=False):
        """
        process run
        :param to_excel:
        :return:
        """
        jobs = list()
        names = list()
        excel_datas = list()
        for k, v in PUBLIC_URL_LIST.iteritems():
            if not k or not v:
                continue
            names.append(k)
            jobs.append(gevent.spawn(self._deal_url, k, v))
        gevent.joinall(jobs)
        for name, job in zip(names, jobs):
            value = job.value
            print '==================%s==================' % name
            print 'Title: %s' % value.get('title')
            print 'Keyword: %s' % value.get('keyword')
            print 'Description: %s' % value.get('description')
            value['name'] = name
            excel_datas.append(value)
    
        self.to_generate_kw(excel_datas)
    
        if to_excel:
            print '---------excel ok'
            self.to_excel(excel_datas, 'companys.xls')


if __name__ == '__main__':
    companyGrap().run(to_excel=False)
{% endcode %}
