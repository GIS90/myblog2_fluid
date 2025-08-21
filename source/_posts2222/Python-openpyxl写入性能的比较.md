---
title: openpyxl写入性能的比较
index_img: /img_index/index/20210808-001.jpg
categories:
  - Python
tags:
  - Python
  - Excel
desc: 比较一下openpyxl在写入表格的性能对比
keywords: 'Python, openpyxl, Excel, 性能, 比较'
abbrlink: 24240
date: 2021-08-08 11:47:56
updated: 2021-08-08 11:47:56
---

<font size=6.5 color='red'>简单比较一下openpyxl在行写入与单元格写入性能上的对比。</font>

<hr />
{% label info@Python %} {% label primary@Excel %}

<!--more-->
<hr />

#### 简述

之前一直用xlrd、xlwt处理表格，但是发现限制台，超过了65535就不能读取了，改用openpyxl，了解了一下之后，发现openpyxl可以行写入，测试一下行写入与单元格写入二者的性能。

#### 样本

表格元数据176233行、7列，数据包含字符串类型、数字、时间等常见类型。

#### 试验体

PC：MacOS
openpyxl版本：3.0.7

#### 源代码

```
import datetime
import openpyxl


# 读取
reader_workbook = openpyxl.load_workbook('data/LLBB_GRCKYE1.xlsx')
sheet = reader_workbook.get_sheet_by_name(reader_workbook.sheetnames[0])
sheet_row = sheet.max_row
sheet_col = sheet.max_column
# ===================================
# 行写入
start1 = datetime.datetime.now()
new_wb = openpyxl.Workbook()
new_sheet = new_wb.create_sheet(title='Sheet', index=0)
for row in sheet.values:
    new_sheet.append(row)
new_wb.save('result/res2.xlsx')
end1 = datetime.datetime.now()
print((end1-start1).seconds)
# ===================================
# 单元格写入
start2 = datetime.datetime.now()
new_wb = openpyxl.Workbook()
new_sheet = new_wb.create_sheet(title='Sheet', index=0)
for row in range(1, sheet_row + 1, 1):
    for col in range(1, sheet_col + 1, 1):
        new_sheet.cell(row=row,
                       column=col,
                       value=sheet.cell(row=row, column=col).value
                       )
new_wb.save('result/res3.xlsx')
end2 = datetime.datetime.now()
print((end2-start2).seconds)

```

#### 比较结果

单位：s

|  类别  | 第1次 | 第2次 | 第3次 | 第4次 | 第5次 | 第6次 | 第7次 | 第8次 | 第9次 | 第10次 | 平均值 |
|:------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:------:|:------:|
|   行   |  25   |  26   |  28   |  29   |  27   |  28   |  27   |  26   |  26   |   26   |  26.8  |
| 单元格 |  26   |  28   |  30   |  28   |  30   |  29   |  27   |  27   |  26   |   29   |  28.0  |

#### 总结

总体来说，大数据量可以看出行写入的速率较快，如果是数据量在几万行，性能都差不多，而且行写入代码也比较简单。
