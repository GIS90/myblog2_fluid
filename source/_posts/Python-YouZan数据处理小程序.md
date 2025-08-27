---
title: Python-YouZan数据处理小程序
index_img: /img_index/index/20200214-001.jpg
categories:
  - Python
tags:
  - Python
  - Python程序篇
keywords: 'python, api, 程序, YouZan, 数据, 脚本, 程序, 数据库, Excel, CSV'
abbrlink: 24462
date: 2020-02-14 16:47:27
updated: 2020-02-14 16:47:27
desc: 获取YouZan的数据进行db或excel存储
---

{% label primary@Python实战 %} {% label success@数据存储 %}

#### 背景
{% note primary %}
前几天朋友找我问我能不能把人家接口数据存下来，大致需求就是获取YouZan的数据进行存储，存到excel，方便使用。花了大概2days的时间，连查询YouZan官方的API，以及对数据进行一些确认处理，写完了记录下来发布到博客，<font color='red' size=4.5>如果有需求的人，拿走不谢</font>！！！。
{% endnote %}


<!--more-->
<hr />

#### 1、环境

| id  |  name  | Version |
|:---:|:------:|:-------:|
|  1  | Python |   2.7   |

#### 2、数据流

<font color='red' size=5>YouZan -> 获取数据 -> 处理数据 ->存储</font>

#### 3、执行方法
```
python2 get_youzan_task.py
```

#### 4、存储方式

```
# 数据库
youzan.run(_type=1)
# Excel
youzan.run(_type=2)
```

#### 5、代码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------
describe:
    先获取可以获取数据的token
    以递增的频率方式进行获取全量数据
    把数据存储到excel或者数据库

usage:
    python get_youzan.py


base_info:
    __version__ = "v.10"
    __author__ = "mingliang.gao"
    __time__ = "2019/9/6"
    __mail__ = "mingliang.gao@qunar.com"


create table sql:
CREATE TABLE `blog`.`client_new` (
    `id` bigint AUTO_INCREMENT NOT NULL,
    `created_at` timestamp NULL COMMENT '\n成为客户的时间，时间戳格式，单位秒\n成为客户的时间\n成为会员的时间',
    `is_member` tinyint COMMENT '是否是会员\n是否是会员',
    `name` varchar(254) COMMENT '客户姓名',
    `show_name` varchar(254) COMMENT '推荐展示姓名',
    `mobile` varchar(30) COMMENT '手机号',
    `member_created_at` timestamp NULL COMMENT '\n成为会员的时间，时间戳格式，单位秒',
    PRIMARY KEY (`id`)
) COMMENT='';

insert test sql:
insert into client(weixin_fans_id, fans_id, created_at, gender, is_member, trade_count, show_name, name, yz_uid, points, mobile, member_created_at)
            VALUES(1111111, 111111, "2020-10-10 00:00:00", "1", 1, 2000, "测试22222", "测试1111", 22222222, 123123, "123123123123", "2020-10-10 00:00:00");
------------------------------------------------
"""
import requests
import time
import os
import datetime
import pymysql
import xlwt
from dateutil.relativedelta import relativedelta


# 数据库设置
DB_HOST = '212.64.61.62'
DB_PORT = 3306
DB_USER = 'mingliang.gao'
DB_PWD = 'XXXXXXXXXXXX'
DB_NAME = 'blog'


class YOUZAN_Client(object):
    def __init__(self):
        self.YOUZAN_ROOT_URL = "https://open.youzanyun.com"
        self.YOUZAN_TOKEN_URL = "https://open.youzanyun.com/auth/token"
        self.YOUZAN_CLIENT_URL = "https://open.youzanyun.com/api/youzan.scrm.customer.search/3.1.2"

        # YouZan 客户端ID
        self.CLIENT_ID = 'XXXXXX'
        self.CLIENT_SECRET = 'XXXXXX'
        self.GRANT_ID = 'XXXXXX'

        self.TOTAL = 0

        # 配置数据获取的开始时间、结束时间、排除时间
        self.START_DATETIME = '2020-02-01 00:00:00'
        self.END_DATETIME = '2020-02-12 10:00:00'
        self.EXCLUDE_DATETIME = [
            '2020-02-06 22:00:00',
            '2020-02-06 23:00:00',
            '2020-02-07 00:00:00',
            '2020-02-07 01:00:00',
            '2020-02-07 02:00:00',
            '2020-02-07 03:00:00',
            '2020-02-07 04:00:00',
            '2020-02-07 05:00:00'
        ]
        # 配置excel字段，字段只能从返回数据list中进行选取
        # weixin_fans_id, fans_id, created_at, gender, is_member, trade_count, show_name, name, yz_uid, points, mobile, member_created_at
        self.EXCEL_FIELDS = ["id", "created_at", "is_member", "name", "show_name", 'mobile', 'member_created_at']

        # DB数据库初始化
        self.conn = pymysql.connect(host=DB_HOST,
                                    port=DB_PORT,
                                    user=DB_USER,
                                    password=DB_PWD,
                                    database=DB_NAME,
                                    charset='utf8')

        self.MAX_PAGE_SIZE = 50

    def add_month(self, transfer_dt, num):
        """
        实现时间上的月份相加
        :param transfer_dt:
        :param num:
        :return:
        """
        if isinstance(transfer_dt, str):
            transfer_dt = datetime.datetime.strptime(transfer_dt, "%Y-%m-%d %H:%M:%S")
        return datetime.datetime.strftime(transfer_dt + relativedelta(months=+1), "%Y-%m-%d %H:%M:%S")

    def deal_time(self, transfer_dt, num, type='add', time_type='day'):
        """
        实现时间上的天数相加相减
        :param transfer_dt:
        :param num:
        :return:
        """
        if type not in ['add', 'min']:
            return None
        if time_type not in ['days', 'hours', 'minutes', 'seconds']:
            return None

        num = num if type == 'add' else -num
        if isinstance(transfer_dt, str):
            transfer_dt = datetime.datetime.strptime(transfer_dt, "%Y-%m-%d %H:%M:%S")

        if time_type == 'day':
            return (transfer_dt + datetime.timedelta(days=num)).strftime("%Y-%m-%d %H:%M:%S")
        elif time_type == 'hours':
            return (transfer_dt + datetime.timedelta(hours=num)).strftime("%Y-%m-%d %H:%M:%S")
        elif time_type == 'minutes':
            return (transfer_dt + datetime.timedelta(minutes=num)).strftime("%Y-%m-%d %H:%M:%S")
        elif time_type == 'seconds':
            return (transfer_dt + datetime.timedelta(seconds=num)).strftime("%Y-%m-%d %H:%M:%S")

    def _check_token_expire(self, token_time):
        """
        check token is or expire
        :param token_time:
        :return:
        """
        if not token_time:
            return False, '过期时间为null'

        # 获取的时间为毫秒
        token_date_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(token_time/1000))
        now_time = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        return (True, token_date_time) if now_time < token_date_time \
            else (False, token_date_time)

    def get_token(self):
        """
        get available token from youzan
        :return:
        """
        payload = {
            "client_id": self.CLIENT_ID,
            "client_secret": self.CLIENT_SECRET,
            "authorize_type": "silent",
            "grant_id": self.GRANT_ID
        }
        headers = {
            'Content-Type': "application/json",
        }

        try:
            token_resp = requests.post(url=self.YOUZAN_TOKEN_URL,
                                       headers=headers,
                                       json=payload)
            if token_resp.status_code != 200:
                return False, 'token请求失败'
            token_resp_json = token_resp.json()
            if not token_resp_json:
                return False, 'token无json数据'

            is_ok = token_resp_json.get('success')
            if not is_ok:
                return False, 'token json success为false'

            token_time = token_resp_json.get('data').get('expires')
            is_ok, token_expires = self._check_token_expire(token_time)
            if not is_ok:
                return False, 'token过期：%s' % token_expires

            return True, token_resp_json.get('data').get('access_token')
        except Exception as e:
            return False, 'token请求Exception失败: %s' % e.message

    def _transfer_to_time(self, timeStamp, t_type=1):
        """
        type=1 把时间戳转换成datetime str类型
        type=2 把datetime类型/datetime str类型转换成时间戳

        :param timeStamp: dateTime or timeStamp
        :param t_type: 1 or 2
        :return:
        """
        if t_type == 2:
            if isinstance(timeStamp, str):
                timeStamp = time.strptime(timeStamp, "%Y-%m-%d %H:%M:%S")
            return int(time.mktime(timeStamp))

        if isinstance(timeStamp, str):
            timeStamp = int(timeStamp)
        return time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(timeStamp))

    def set_style(self, name, height, bold=False):
        style = xlwt.XFStyle()
        font = xlwt.Font()
        font.name = name
        font.bold = bold
        font.color_index = 4
        font.height = height
        style.font = font
        return style

    def _deal_client_to_excel(self, datas):
        """
        :param datas: client object to excel
        :return: None
        """
        if not datas:
            return

        f = xlwt.Workbook(encoding='utf-8')
        sheet = f.add_sheet('sheet', cell_overwrite_ok=True)

        title_style = self.set_style('Times New Roman', 220, True)
        content_style = self.set_style('Times New Roman', 220, False)
        # 标题
        for i in range(0, len(self.EXCEL_FIELDS)):
            sheet.write(0, i, self.EXCEL_FIELDS[i], title_style)

        row = 1

        for data in datas:
            if not data:
                continue

            weixin_fans_id = data.get('weixin_fans_id')
            fans_id = data.get('fans_id')
            created_at = self._transfer_to_time(data.get('created_at')) \
                if data.get('created_at') else ''
            member_created_at = self._transfer_to_time(data.get('member_created_at')) \
                if data.get('created_at') else ''
            gender = data.get('gender')
            is_member = data.get('is_member')
            trade_count = data.get('trade_count')
            show_name = data.get('show_name')
            name = data.get('name')
            yz_uid = data.get('yz_uid')
            points = data.get('points')
            mobile = data.get('mobile')

            if isinstance(show_name, unicode):
                show_name = show_name.encode('utf-8')
            if isinstance(mobile, unicode):
                mobile = mobile.encode('utf-8')
            if isinstance(created_at, unicode):
                created_at = created_at.encode('utf-8')
            if isinstance(member_created_at, unicode):
                member_created_at = member_created_at.encode('utf-8')
            if isinstance(name, unicode):
                name = name.encode('utf-8')
            if isinstance(mobile, unicode):
                mobile = mobile.encode('utf-8')

            for field in self.EXCEL_FIELDS:
                if field == 'id':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), row, title_style)
                elif field == 'weixin_fans_id':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), weixin_fans_id, content_style)
                elif field == 'fans_id':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), fans_id, content_style)
                elif field == 'created_at':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), created_at, content_style)
                elif field == 'member_created_at':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), member_created_at, content_style)
                elif field == 'gender':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), gender, content_style)
                elif field == 'is_member':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), is_member, content_style)
                elif field == 'trade_count':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), trade_count, content_style)
                elif field == 'show_name':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), show_name, content_style)
                elif field == 'name':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), name, content_style)
                elif field == 'yz_uid':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), yz_uid, content_style)
                elif field == 'points':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), points, content_style)
                elif field == 'mobile':
                    sheet.write(row, self.EXCEL_FIELDS.index(field), mobile, content_style)
            row += 1

        f.save('%s-%s名单.xls' % (self.START_DATETIME, self.END_DATETIME))

    def _deal_client_to_db(self, data):
        """
        :param data: client object to database
        :return: None
        """
        if not data:
            return

        weixin_fans_id = data.get('weixin_fans_id')
        fans_id = data.get('fans_id')
        created_at = self._transfer_to_time(data.get('created_at')) \
            if data.get('created_at') else ''
        member_created_at = self._transfer_to_time(data.get('member_created_at')) \
            if data.get('created_at') else ''
        gender = data.get('gender')
        is_member = data.get('is_member')
        trade_count = data.get('trade_count')
        show_name = data.get('show_name')
        name = data.get('name')
        yz_uid = data.get('yz_uid')
        points = data.get('points')
        mobile = data.get('mobile')

        if isinstance(show_name, unicode):
            show_name = show_name.encode('utf-8')
        if isinstance(mobile, unicode):
            mobile = mobile.encode('utf-8')
        if isinstance(created_at, unicode):
            created_at = created_at.encode('utf-8')
        if isinstance(member_created_at, unicode):
            member_created_at = member_created_at.encode('utf-8')
        if isinstance(name, unicode):
            name = name.encode('utf-8')
        if isinstance(mobile, unicode):
            mobile = mobile.encode('utf-8')

        # insert_sql = """
        # insert into client(weixin_fans_id, fans_id, created_at, gender, is_member, trade_count, show_name, name, yz_uid, points, mobile, member_created_at)
        # VALUES(%s, %s, "%s", "%s", %s, %s, "%s", "%s", %s, %s, "%s", "%s");
        # """ % (weixin_fans_id, fans_id, created_at, gender, is_member, trade_count, show_name, name, yz_uid, points, mobile, member_created_at)
        insert_sql = 'insert into client_new(created_at, is_member, name, show_name, mobile, member_created_at) VALUES(%s, %s, %s, %s, %s, %s);'
        cursor = self.conn.cursor()
        cursor.execute(insert_sql, (created_at, is_member, name, show_name, mobile, member_created_at))
        self.conn.commit()
        cursor.close()

    def deal_client_by_token(self, token, store):
        """
        main method by token
        设置一个起始时间，每次递增加1月进行数据获取，以此方式进行all data获取
        :param token:
        :param store: db or excel
        :return:
        """

        all_datas = list()
        end_time = self.START_DATETIME

        while True:
            start_time = end_time
            if start_time == self.END_DATETIME:
                break

            end_time = self.deal_time(start_time, 1, 'add', 'hours')
            # 去掉脏数据时间段
            if start_time in self.EXCLUDE_DATETIME:
                print '【%s】is exclude datetime' % start_time
                continue

            payload = {
                # "is_member": 1,   # 是否会员
                "created_at_start": self._transfer_to_time(start_time, 2),     # 成为客户的时间
                "created_at_end": self._transfer_to_time(end_time, 2)     # 成为客户的时间
            }
            headers = {
                'Content-Type': "multipart/form-data",
            }
            real_url = '%s?access_token=%s' % (self.YOUZAN_CLIENT_URL, token)
            data_resp = requests.post(url=real_url,
                                      # headers=headers,
                                      data=payload)

            if data_resp.status_code != 200:
                print '【%s】client请求失败' % start_time
                continue
            resp_json_datas = data_resp.json()
            if not resp_json_datas:
                print '【%s】client无json数据' % start_time
                continue
            code = resp_json_datas.get('code')
            if code != 200:
                print '【%s】client code is error: %s' % (start_time, code)
                continue
            total = resp_json_datas.get('data').get('total')
            if total < 1:
                print '【%s】client json数据total: %s' % (start_time, total)
                continue

            # 特别说2 = 1 + 1，有一个page页面的1，还有一个range的1
            page = int(total / 50) + 2
            self.TOTAL = self.TOTAL + total
            print '%s - %s total: %s, page: %s' \
                  % (start_time, end_time, total, (page - 1))

            for i in range(1, page, 1):
                print '===========%s' % i
                sub_payload = {
                    "created_at_start": self._transfer_to_time(start_time, 2),     # 成为客户的时间
                    "created_at_end": self._transfer_to_time(end_time, 2),     # 成为客户的时间
                    "page_size": self.MAX_PAGE_SIZE     # 每页数量，最多支持50个
                }
                sub_data_resp = requests.post(url=real_url,
                                              data=sub_payload)
                sub_resp_json_datas = sub_data_resp.json()
                datas = sub_resp_json_datas.get('data').get('record_list')
                for data in datas:
                    if not data:
                        continue
                    self._deal_client_to_db(data) if store == 'db' \
                        else all_datas.append(data)

        if store == 'excel':
            self._deal_client_to_excel(all_datas)

    def run(self, _type=1):
        store = 'db' if _type == 1 else 'excel'
        is_ok, message = self.get_token()
        if not is_ok:
            print 'YouZan token获取失败：【%s】' % message
            os._exit()

        print 'token: %s' % message
        self.deal_client_by_token(message, store)
        if store == 'db':
            youzan.conn.close()
        print '===================end: %s' % self.TOTAL


if __name__ == '__main__':

    youzan = YOUZAN_Client()
    youzan.run(_type=2)
```

![](py.jpeg)
