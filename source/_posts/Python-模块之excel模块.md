---
title: Python模块之excel模块
index_img: /img_index/index/article_python_module.jpeg
categories:
  - [Python]
tags: [Python, Python模块系列]
abbrlink: 19275
date: 2022-08-13 22:20:32
updated: 2022-08-13 22:20:32
desc: Python模块之excel模块
keywords: Python
---

{% raw %}
<div class="post_cus_note">Python模块系列</div>
{% endraw %}

<!--more-->
<hr />




#### 版本

|  名称  | 版本 |
|:------:|:----:|
| Python |  3   |


#### 描述

Excel表读取、写入工具
使用了xlrd、xlwt，Excel表格处理包进行开发的lib工具包
- read

- write
其中，在写入模块加入了表格样式处理，使其生成的表格自带cell样式，更加美观

#### 设计

利用Python包开发的表格工具包。
- xlrd: 表格读取包
- xlwt: 表格写入包


#### 使用

```
excel_lib = ExcelLib()
# 读取
excel_lib.read(read_file)
# 写入
excel_res = self.excel_lib.beau_write(excel_file=excel_file, data=inspect_list)
```

#### 源码

```
# -*- coding: utf-8 -*-

"""
------------------------------------------------

describe:
    Excel表读取、写入工具
    使用了xlrd、xlwt，Excel表格处理包进行开发的lib工具包
    - read
    - write
    其中，在写入模块加入了表格样式处理，使其生成的表格自带cell样式，更加美观

base_info:
    __author__ = "PyGo"
    __time__ = "2022/9/15"
    __version__ = "v.1.0.0"
    __mail__ = "gaoming971366@163.com"
    __project__ = "quality-inspect"

usage:
    excel_lib = ExcelLib()
    # 读取
    excel_lib.read(read_file)
    # 写入
    excel_res = self.excel_lib.beau_write(excel_file=excel_file, data=inspect_list)

design:
    - xlrd: 表格读取包
    - xlwt: 表格写入包
    利用Python包开发的表格工具包。


reference urls:

python version:
    python3


Enjoy the good time everyday！！!
Life is short, I use python.

------------------------------------------------
"""

# ------------------------------------------------------------
# usage: /usr/bin/python excel_lib.py
# ------------------------------------------------------------
import os
import xlwt
import xlrd

from deploy.utils import get_now, mk_dirs
from deploy.status_msg import StatusMsgs
from config import RESULT_EXCEL, DEBUG
from deploy.logger import logger as LOG


class ExcelLib(object):
    """
    Excel class
    """

    # 类常量参数
    DEFAULT_OLD_V_PREFIX = '.xls'
    DEFAULT_NEW_V_PREFIX = '.xlsx'
    DEFAULT_SHEET_NAME = 'Sheet1'

    # 表格样式 > 文字横向方向
    HORZ_DICT = {
        'left': 0x01,
        'center': 0x02,
        'right': 0x03
    }
    # 表格样式 > 文字纵向方向
    VERT_DICT = {
        'top': 0x00,
        'center': 0x01,
        'bottom': 0x02
    }
    # 表格样式 > 单元格边框
    BORDER_LINES = {
        'NO_LINE': xlwt.Borders.NO_LINE,
        'THIN': xlwt.Borders.THIN,
        'MEDIUM': xlwt.Borders.MEDIUM,
        'DASHED': xlwt.Borders.DASHED,
        'DOTTED': xlwt.Borders.DOTTED,
        'THICK': xlwt.Borders.THICK,
        'DOUBLE': xlwt.Borders.DOUBLE,
        'HAIR': xlwt.Borders.HAIR
    }
    # 表格样式 > 单元格类型
    FORMATTER = [
        'general',
        '0',
        '0.00',
        '#,##0',
        '#,##0.00',
        '"$"#,##0_);("$"#,##',
        '"$"#,##0_);[Red]("$"#,##',
        '"$"#,##0.00_);("$"#,##',
        '"$"#,##0.00_);[Red]("$"#,##',
        '0%',
        '0.00%',
        '0.00E+00',
        '# ?/?',
        '# ??/??',
        'M/D/YY',
        'D-MMM-YY',
        'D-MMM',
        'MMM-YY',
        'h:mm AM/PM',
        'h:mm:ss AM/PM',
        'h:mm',
        'h:mm:ss',
        'M/D/YY h:mm',
        '_(#,##0_);(#,##0)',
        '_(#,##0_);[Red](#,##0)',
        '_(#,##0.00_);(#,##0.00)',
        '_(#,##0.00_);[Red](#,##0.00)',
        '_("$"* #,##0_);_("$"* (#,##0);_("$"* "-"_);_(@_)',
        '_(* #,##0_);_(* (#,##0);_(* "-"_);_(@_)',
        '_("$"* #,##0.00_);_("$"* (#,##0.00);_("$"* "-"??_);_(@_)',
        '_(* #,##0.00_);_(* (#,##0.00);_(* "-"??_);_(@_)',
        'mm:ss',
        '[h]:mm:ss',
        'mm:ss.0',
        '##0.0E+0',
        '@'
    ]

    def __init__(self, blank=0):
        """
        class initialize parameters
        :param blank: blank number
        """
        if DEBUG:
            LOG.debug('Excel lib is loading.')
        self.prefix_list = ['.xlsx', '.xls']
        self.blank = 0  # 空行个数，默认值0行

    def set_style(self, font='Times New Roman', size=11, color=0, bgcolor='white', horz='left', vert='center', **kwargs):
        """
        set the excel cell style, to achieve customize style.
        all parameters have default value.
        parameters detail informations refer to the follow description.
        :param font: font type
            default value is "Times New Roman"
            currently, the font is not use, font type function will be added later
        :param size: font size
            default value is 11
        :param color: font color index
            color value range is 0 ~ 100, default value is black(0)
            this is only list 1-7:
            0 黑 black
            1 白 white
            2 红 red
            3 酸橙 lime
            4 蓝 blue
            5 黄 yellow
            6 洋红 magenta
            7 青 cyan
        :param bgcolor: cell background
            this value is english color, default value is white, color range:
            aqua 0x31
            black 0x08
            blue 0x0C
            blue_gray 0x36
            bright_green 0x0B
            brown 0x3C
            coral 0x1D
            cyan_ega 0x0F
            dark_blue 0x12
            dark_blue_ega 0x12
            dark_green 0x3A
            dark_green_ega 0x11
            dark_purple 0x1C
            dark_red 0x10
            dark_red_ega 0x10
            dark_teal 0x38
            dark_yellow 0x13
            gold 0x33
            gray_ega 0x17
            gray25 0x16
            gray40 0x37
            gray50 0x17
            gray80 0x3F
            green 0x11
            ice_blue 0x1F
            indigo 0x3E
            ivory 0x1A
            lavender 0x2E
            light_blue 0x30
            light_green 0x2A
            light_orange 0x34
            light_turquoise 0x29
            light_yellow 0x2B
            lime 0x32
            magenta_ega 0x0E
            ocean_blue 0x1E
            olive_ega 0x13
            olive_green 0x3B
            orange 0x35
            pale_blue 0x2C
            periwinkle 0x18
            pink 0x0E
            plum 0x3D
            purple_ega 0x14
            red 0x0A
            rose 0x2D
            sea_green 0x39
            silver_ega 0x16
            sky_blue 0x28
            tan 0x2F
            teal 0x15
            teal_ega 0x15
            turquoise 0x0F
            violet 0x14
            white 0x09
            yellow 0x0D
        :param horz: cell horizontal（水平方向上）
            default value is left
            left = 0x01 左端对齐
            center = 0x02 居中对齐
            right = 0x03 右端对齐
            if horz not in values dict, use default value(left)
        :param vert: cell vertical（垂直方向上）
            default value is center
            top = 0x00 上端对齐
            center = 0x01 居中对齐
            bottom = 0x02 低端对齐
            if vert not in values dict, use default value(center)
        :param kwargs: other parameters, contain: bold, underline, italic, width, height, borders
            bold: font bold
                default is font no bold
                value range is: True or False, default value is False
            underline: font underline
                default is font no underline
                value range is: True or False, default value is False
            italic: font italic
                default is font no italic
                value range is: True or False, default value is False
            struck_out: font struck_out 字体删除符，字上加一横杠
                default is font no struck_out
                value range is: True or False, default value is False
            width: cell width
                default value is cell default width
            height: cell height
                default value is cell default height
            borders: cell borders, contain left, right, top, bottom
                default value is cell no borders
                value range is True or False
                border line, default is NO_LINE:
                    NO_LINE = 0x00
                    THIN    = 0x01
                    MEDIUM  = 0x02
                    DASHED  = 0x03
                    DOTTED  = 0x04
                    THICK   = 0x05
                    DOUBLE  = 0x06
                    HAIR    = 0x07
                border color, default is 0x40.
            borders_dict: customize to set border style
                line: left, right, top, bottom
                color: left, right, top, bottom
                data frame:
                    borders_dict = {
                        'line': {
                            'top': 'THIN',
                            'bottom': 'THIN',
                            'left': 'THIN',
                            'right': 'THIN'
                        },
                        'color': {
                            'top': '0x40',
                            'bottom': '0x40',
                            'left': '0x40',
                            'right': '0x40'
                        },
                    }
            warp: is or not auto wrap 自动换行
                default value is False,
                if True, cell value is auto wrap
                if False, cell value is not warp
            formatter: cell formatter
                contain numbers, string, date, time and so on.
                default value is general
                detail scan FORMATTER values
                    'general',
                    '0',
                    '0.00',
                    '#,##0',
                    '#,##0.00',
                    '"$"#,##0_);("$"#,##',
                    '"$"#,##0_);[Red]("$"#,##',
                    '"$"#,##0.00_);("$"#,##',
                    '"$"#,##0.00_);[Red]("$"#,##',
                    '0%',
                    '0.00%',
                    '0.00E+00',
                    '# ?/?',
                    '# ??/??',
                    'M/D/YY',
                    'D-MMM-YY',
                    'D-MMM',
                    'MMM-YY',
                    'h:mm AM/PM',
                    'h:mm:ss AM/PM',
                    'h:mm',
                    'h:mm:ss',
                    'M/D/YY h:mm',
                    '_(#,##0_);(#,##0)',
                    '_(#,##0_);[Red](#,##0)',
                    '_(#,##0.00_);(#,##0.00)',
                    '_(#,##0.00_);[Red](#,##0.00)',
                    '_("$"* #,##0_);_("$"* (#,##0);_("$"* "-"_);_(@_)',
                    '_(* #,##0_);_(* (#,##0);_(* "-"_);_(@_)',
                    '_("$"* #,##0.00_);_("$"* (#,##0.00);_("$"* "-"??_);_(@_)',
                    '_(* #,##0.00_);_(* (#,##0.00);_(* "-"??_);_(@_)',
                    'mm:ss',
                    '[h]:mm:ss',
                    'mm:ss.0',
                    '##0.0E+0',
                    '@'

        :return: object

        usge:
            column_style = self.set_style(size=8, horz='center', vert='center', color=0, bgcolor='ice_blue', borders=True)
            sheet.write(row, col, JG_ENUMS.get(jg), column_style)
        """
        style = xlwt.XFStyle()
        font = xlwt.Font()
        # font: type
        # font.name = font or 'Times New Roman'
        # font: size
        try:
            size = int(size)
        except:
            size = 11
        font.height = size * 20
        # font: color
        try:
            color = int(color)
        except:
            color = 0
        finally:
            if color > 100: color = 0
        font.colour_index = color
        # font: bold
        bold = kwargs.get('bold') or False
        if bold not in [True, False]:
            bold = False
        font.bold = bold
        # font: underline
        underline = kwargs.get('underline') or False
        if underline not in [True, False]:
            underline = False
        font.underline = underline
        # font: italic
        italic = kwargs.get('italic') or False
        if italic not in [True, False]:
            italic = False
        font.italic = italic
        # font: struck_out
        struck_out = kwargs.get('struck_out') or False
        if struck_out not in [True, False]:
            struck_out = False
        font.struck_out = struck_out
        style.font = font

        # align: horz
        horz_v = self.HORZ_DICT.get(horz.lower())
        if not horz_v: horz_v = 0x02
        # align: vert
        vert_v = self.VERT_DICT.get(vert.lower())
        if not vert_v: vert_v = 0x01
        align = xlwt.Alignment()
        align.horz = horz_v
        align.vert = vert_v
        # align: wrap
        wrap = kwargs.get('wrap') or False
        if wrap not in [True, False]:
            wrap = False
        align.wrap = wrap
        style.alignment = align

        # background color
        pattern = xlwt.Pattern()
        pattern.pattern = xlwt.Pattern.SOLID_PATTERN
        try:
            pattern.pattern_fore_colour = xlwt.Style.colour_map[bgcolor]
        except:
            pattern.pattern_fore_colour = xlwt.Style.colour_map['white']
        style.pattern = pattern

        # borders
        is_border = kwargs.get('borders') or False
        if is_border not in [True, False]:
            is_border = False
        if is_border:
            border = xlwt.Borders()
            border.top = xlwt.Borders.THIN  # 上
            border.bottom = xlwt.Borders.THIN  # 下
            border.left = xlwt.Borders.THIN  # 左
            border.right = xlwt.Borders.THIN  # 右
            border.left_colour = 0x40
            border.right_colour = 0x40
            border.top_colour = 0x40
            border.bottom_colour = 0x40
            style.borders = border
        borders_dict = kwargs.get('borders_dict') or False
        if borders_dict:
            border = xlwt.Borders()
            for k, v in borders_dict.get('line').items():
                if k == 'top':
                    border.top = self.BORDER_LINES.get(v.upper()) if v.upper() in self.BORDER_LINES.keys() \
                        else xlwt.Borders.NO_LINE
                elif k == 'bottom':
                    border.bottom = self.BORDER_LINES.get(v.upper()) if v.upper() in self.BORDER_LINES.keys() \
                        else xlwt.Borders.NO_LINE
                elif k == 'left':
                    border.left = self.BORDER_LINES.get(v.upper()) if v.upper() in self.BORDER_LINES.keys() \
                        else xlwt.Borders.NO_LINE
                elif k == 'right':
                    border.right = self.BORDER_LINES.get(v.upper()) if v.upper() in self.BORDER_LINES.keys() \
                        else xlwt.Borders.NO_LINE
            border.left_colour = 0x40
            border.right_colour = 0x40
            border.top_colour = 0x40
            border.bottom_colour = 0x40
            style.borders = border

        # formatter
        num_format_str = kwargs.get('formatter') or 'general'
        if num_format_str not in self.FORMATTER:
            num_format_str = 'general'
        style.num_format_str = num_format_str
        return style

    @staticmethod
    def format_res(status_id: int, message: str, data: dict) -> dict:
        """
        方法请求结果格式化
        """
        return {
            'status_id': status_id,
            'message': message if message else StatusMsgs.get(status_id),
            'data': data
        }

    def read_headers(self, excel_file):
        """
        get the headers information of excel file
        :param excel_file: excel file real path
        :return: dict
        result content:
            sheets: dict sheet base info, contain something. such as row.col.index.name
            number sheet: int sheet number
            names: sheets name dict
            columns: sheet columns dict
        """
        # default value
        res = {'sheets': {}, 'nsheet': 0, 'names': {}, 'columns': {}}
        # not exist excel file
        if not excel_file \
                or not os.path.exists(excel_file):
            return res

        excel = xlrd.open_workbook(excel_file)
        nsheets = excel.nsheets
        # sheet_names = excel.sheet_names()     # 获取所有excel sheet名称列表
        sheets_dict = dict()
        names_dict = dict()
        columns_dict = dict()
        for i in range(0, nsheets, 1):
            sheet = excel.sheet_by_index(i)
            names_dict[str(i)] = sheet.name
            # sheet：row行数 col列数 index索引 name名称
            sheets_dict[str(i)] = {'row': sheet.nrows, 'col': sheet.ncols, 'index': i, 'name': sheet.name}
            try:
                # sheet column：key：value格式
                row_values_0 = sheet.row_values(0)
                new_column = list()
                for _k, _v in enumerate(row_values_0):
                    new_column.append({'key': str(_k), 'value': str(_v)})
                columns_dict[str(i)] = new_column
            except:
                pass
        return {'sheets': sheets_dict, 'nsheet': nsheets, 'names': names_dict, 'columns': columns_dict}

    def check_real_file(self, real_file: str, _type=1, clean=False):
        """
        生成存储excel文件的绝对路径，以及新文件名称
        加了文件已存在的check
        结果采用绝对路径+新文件名称，新文件名称
        如果存在用新的时间戳创建文件
        generate to store real path of excel file
        if file exist dir, the file name rename file name + now(%Y-%m-%d-%H-%M-%S)
        the result is path file
        :param clean: 是否删除已存在
        :param real_file: all path excel file
        :param _type: excel file version type
        :return: sting

        _type为默认格式：
            - 1为.xls
            - 2为.xlsx
        """
        if not _type or _type not in [1, 2]:    # 默认xls
            _type = 1

        # 存在 && 删除
        if clean and os.path.exists(real_file):
            os.remove(real_file)
            return real_file

        # 文件已存在，加上时间戳
        if os.path.exists(real_file):
            file_name = '%s%s' % (get_now(format="%Y-%m-%d-%H-%M-%S"), self.DEFAULT_OLD_V_PREFIX)
            real_file = os.path.join(self._default_folder(), file_name)
            return real_file
        return real_file

    @staticmethod
    def _default_folder():
        """
        new excel file folder
        :return: string
        """
        if not os.path.exists(RESULT_EXCEL):
            mk_dirs(RESULT_EXCEL)
        return RESULT_EXCEL

    def read(self, read_file: str, sheet: int = 0, rows: list = [], columns: list = [], **kwargs) -> dict:
        """
        read excel data
        :param read_file: excel文件abs全路径
        :param sheet: 读取的sheet数，从0开始，default value is 0
        :param rows: 读取数据的rows行列表，如果为空，默认读取全部行
        :param columns: 读取数据的columns列列表，如果为空，默认读取全部列
        :param kwargs: 读取excel数据的其他参数配置
            request_title: 读取的表格是否包含title行，默认是true（bool类型）
            response_title: 返回的数据是否包含title说明，默认是true（bool类型）

        :return: dict result
        """
        # ================== parameters check start ==================
        if not read_file or not os.path.exists(read_file) \
                or not os.path.isfile(read_file):
            return self.format_res(
                206, '读取的excel数据不存在', {})
        request_title = False if kwargs.get('request_title') is False else True
        # 初始化数据读取开始的行数
        start_row = 1 if request_title else 0
        response_title = False if kwargs.get('response_title') is False else True
        excel_object = xlrd.open_workbook(filename=read_file)   # xlrd可以读取xls、xlsx
        excel_sheet_names = excel_object.sheet_names()
        if not str(sheet):          # 添加无sheet index，默认读取首页
            sheet = 0
        # 添加sheet为整型处理
        try:
            if not isinstance(sheet, int):
                sheet = int(sheet)
        except:
            sheet = 0
        if sheet > len(excel_sheet_names) or sheet < 0:
            return self.format_res(
                207, '读取的sheet页不存在', {})
        excel_sheet = excel_object.sheet_by_index(sheet)
        # 读取指定行
        new_rows = list()
        if rows:
            for r in rows:
                try:
                    new_rows.append(int(r))
                except: pass
        # 读取指定列
        new_cols = list()
        if columns:
            for c in columns:
                try:
                    new_cols.append(int(c))
                except: pass
        # ================== parameters check end ==================

        # 指定行
        read_rows = new_rows if new_rows else range(start_row, excel_sheet.nrows, 1)
        # 指定列
        read_cols = new_cols if new_cols else range(0, excel_sheet.ncols, 1)
        # 读取表头
        resp_header = list()
        if response_title and request_title:
            for col in read_cols:
                if col < 0: continue
                # TODO 是否添加空标题、重复标题等标题判断
                # 目前，定位空活着列值重复均可以
                # if not excel_sheet.cell_value(0, col) or excel_sheet.cell_value(0, col) in resp_header:
                #     return self.format_res(
                #         208, '读取的第%s列值为空' % col, {})
                resp_header.append(excel_sheet.cell_value(0, col))
        # 读取表数据
        resp_data = list()
        for row in read_rows:
            if not row: continue
            _d = list()
            for col in read_cols:
                if col < 0: continue
                _d.append(excel_sheet.cell_value(row, col))
            if _d: resp_data.append(_d)
        return self.format_res(
            100, 'success', {'header': resp_header, 'data': resp_data})

    def write(self, excel_file, data, sheet="Sheet1", **kwargs):
        """
        写入的通用方法，xlwt、xlrd:
            .xls表格行数限制65535
        写入的数据为list格式，里面的元素也为list类型数据，以元素list顺序进行表格row的写入

        use xlwt、xlrd to merge excel, it are to deal .xls、.xlsx formatter excel file
        the deal excel style is cell values(row + col)
        :param excel_file: excel file, if not exist to create
        :param data: excel data
        :param sheet: excel sheet name
        :param kwargs: multiple parameters, is dict type
            - title: 是否为标题，如果是标题样式采用标题样式
            - start_row: 写入excel的起始行，默认为0
            - *** extra parameters ***
        :return: json object
            status_id: result id, except status_id is 1 is success, others is failure
            message: the result message
            data:
                path: the new excel path + name, real path
                name: the new excel file name, not contain path
        """
        # if not excel file, auto rename by timestamp
        # new excel file format xlrd xlwt: xls
        if not excel_file:
            return self.format_res(214, '请求参数为file必须信息', {})
        if not data:
            return self.format_res(401, '数据不存在', {})

        try:
            new_excel = xlwt.Workbook(encoding='utf-8')
            new_sheet = new_excel.add_sheet(sheet or self.DEFAULT_SHEET_NAME, cell_overwrite_ok=True)
            start_row = kwargs.get('start_row') or 0    # 写入的起始行
            row = 0
            for _d in data:
                if not _d: continue
                for col in range(0, len(_d), 1):
                    new_sheet.write((start_row + row), col, _d[col])    # 单元格的方式写入
                row += 1
            # excel_file = self.check_real_file(excel_file, clean=True)
            new_excel.save(excel_file)
            return self.format_res(
                100, 'success', {'name': os.path.split(excel_file)[1], 'path': excel_file})
        except Exception as e:
            return self.format_res(998, str(e), {})

    def beau_write(self, excel_file, data, sheet_name="Sheet1", **kwargs):
        """
        针对于质量考核检查结果直接带样式的表格，xlwt、xlrd:
            .xls表格行数限制65535
        特殊方法，写死数据以及样式

        use xlwt、xlrd to merge excel, it are to deal .xls、.xlsx formatter excel file
        the deal excel style is cell values(row + col)
        :param excel_file: excel file, if not exist to create
        :param data: excel data
        :param sheet_name: excel sheet name
        :param kwargs: multiple parameters, is dict type
            - *** extra parameters ***
        :return: json object
            status_id: result id, except status_id is 1 is success, others is failure
            message: the result message
            data:
                path: the new excel path + name, real path
                name: the new excel file name, not contain path
        """
        # if not excel file, auto rename by timestamp
        # new excel file format xlrd xlwt: xls
        if not excel_file:
            return self.format_res(214, '请求参数为Excel file必须信息', {})
        if not data:
            return self.format_res(401, '数据不存在', {})

        try:
            new_excel = xlwt.Workbook(encoding='utf-8')
            new_sheet = new_excel.add_sheet(sheet_name or self.DEFAULT_SHEET_NAME, cell_overwrite_ok=True)

            # initialize cell style
            headline_border = {
                'line': {
                    'top': 'NO_LINE',
                    'bottom': 'DASHED',
                    'left': 'NO_LINE',
                    'right': 'NO_LINE'
                }
            }
            headline_style = self.set_style(size=16, horz='center', vert='center', bold=True,
                                            borders_dict=headline_border, wrap=True)
            title_style = self.set_style(size=10, bold=True, horz='center', vert='center', color=1,
                                         bgcolor='periwinkle', borders=True)
            column_style = self.set_style(size=8, bold=True, horz='center', vert='center', color=0, bgcolor='ice_blue',
                                          borders=True)
            cell_style = self.set_style(size=8, horz='center', vert='center', borders=True)
            cell_per_style = self.set_style(size=8, horz='center', vert='center', borders=True, formatter='0.00%')
            special_cell_style = self.set_style(size=8, horz='center', vert='center', bgcolor='yellow', borders=True)
            special_per_style = self.set_style(size=8, horz='center', vert='center', bgcolor='yellow', borders=True,
                                               formatter='0.00%')
            # excel title
            new_sheet.write_merge(0, 1, 0, 0, "项目编号", title_style)
            new_sheet.write_merge(0, 1, 1, 1, "项目名称", title_style)
            new_sheet.write_merge(0, 0, 2, 6, "测试记录", title_style)
            new_sheet.write_merge(0, 0, 7, 11, "工资确认单", title_style)
            new_sheet.write(1, 2, '提交次数', title_style)
            new_sheet.write(1, 3, '版本', title_style)
            new_sheet.write(1, 4, '用户', title_style)
            new_sheet.write(1, 5, '时间', title_style)
            new_sheet.write(1, 6, '备注', title_style)
            new_sheet.write(1, 7, '提交次数', title_style)
            new_sheet.write(1, 8, '版本', title_style)
            new_sheet.write(1, 9, '用户', title_style)
            new_sheet.write(1, 10, '时间', title_style)
            new_sheet.write(1, 11, '备注', title_style)

            start_row = 2    # 数据写入的起始行
            row = 0
            for _d in data:
                if not _d: continue
                for col in range(0, len(_d), 1):
                    style = column_style if col < 2 else cell_style
                    new_sheet.write((start_row + row), col, _d[col], style)    # 单元格的方式写入
                row += 1
            """
            end write and save
            """

            # 设置列宽, 256为基准数
            for col in [1]:
                new_sheet.col(col).width_mismatch = True
                new_sheet.col(col).width = 256 * 24  # 标准256 * 12
            for col in [5, 6, 10, 11]:
                new_sheet.col(col).width_mismatch = True
                new_sheet.col(col).width = 256 * 18  # 标准256 * 12
            # 设置行高, 20为基准数 50为50磅
            he_rows = [0, 1]
            for row in he_rows:
                new_sheet.row(row).height_mismatch = True  # 标题
                new_sheet.row(row).height = 20 * 20

            # excel_file = self.check_real_file(excel_file, clean=True)
            new_excel.save(excel_file)
            return self.format_res(
                100, 'success', {'name': os.path.split(excel_file)[1], 'path': excel_file})
        except Exception as e:
            return self.format_res(998, str(e), {})
```


#### 系列


[Python模块之command系统命令](http://pygo2.top/articles/26110/)
[Python模块之excel模块](http://pygo2.top/articles/19275/)
[Python模块之logger日志](http://pygo2.top/articles/5145/)
[Python模块之utils公共方法](http://pygo2.top/articles/61799/)
[Python模块之watcher打点](http://pygo2.top/articles/31232/)
[Python模块之config配置解析](http://pygo2.top/articles/51787/)
[Python模块之dtalk钉钉消息](http://pygo2.top/articles/58292/)
[Python模块之企业微信](http://pygo2.top/articles/33254/)

更多模块请参考文章TAG进行查看。

<font size=6.5 color='red'>***Python模块系列，持续更新中。。。。。。***</font>
