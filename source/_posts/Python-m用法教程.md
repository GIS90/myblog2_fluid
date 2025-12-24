---
title: python-m高级用法教程
index_img: /img_index/index/20251224-001.jpg
tags: [Python]
categories: [Python]
abbrlink: 30798
date: 2025-12-24 21:32:28
updated: 2025-12-24 21:32:28
---


在日常工作中，偶尔会看到简易web服务器（**python -m http.server 9999**） 或者CURL请求结果格式化（ **curl XXXX | python -m json.tool**）等命令，看着还挺炫酷的，深度解析一下，并且列举一下常用的python -m命令。


<!--more-->
<hr />


### 1、定义
python -m是一个非常强大的命令行选项，它允许你将模块作为脚本运行，语法糖：
```
python -m module_name [arguments]
```

主要查询的就是模块搜索路径（sys.path）下的模块。


### 2、常见命令

#### 2.1 数据格式处理

- JSON 格式化
```
echo '{"name":"John","age":30}' | python -m json.tool
```

- XML 美化
```
python -m xml.dom.minidom file.xml
```

- CSV 查看
```
python -m csvtool data.csv
```

#### 2.2 网络服务

- 启动简单的 HTTP 服务器
```
python -m http.server 8000  # Python 3
python -m SimpleHTTPServer 8080  # Python 2
```

#### 2.3 开发工具

- Python 代码格式化
```
python -m black script.py
```

- 代码质量检查
```
python -m pylint script.py
python -m flake8 script.py
python -m mypy script.py  # 类型检查
```

- 测试运行器
```
python -m pytest
python -m unittest discover
```

- 性能分析
```
python -m cProfile script.py
```

- 调试器
```
python -m pdb script.py
```

#### 2.4 包管理相关

- 创建虚拟环境
```
python -m venv myenv
```

- pip 工具
```
python -m pip install package_name
python -m pip list
python -m pip freeze > requirements.txt
```

- 打包工具
```
python -m build
```

- 包浏览器
```
python -m site  # 显示 site-packages 路径
```

#### 2.5 系统工具
- 压缩/解压
```
python -m zipfile -c archive.zip file1 file2
python -m zipfile -e archive.zip extract_dir
```

- tar 文件处理
```
python -m tarfile -c archive.tar files/
python -m tarfile -e archive.tar extract_dir/
```

- 计算器
```
python -m calculator
```

- 日历
```
python -m calendar 2024
```

#### 2.6 文本处理
- 编码转换
```
python -m base64 -e "hello"  # 编码
python -m base64 -d "aGVsbG8="  # 解码
```


- 哈希计算
```
echo -n "hello" | python -m hashlib
```

- 正则表达式测试
```
python -m re 'pattern' 'string'
```

#### 2.7 交互式工具
- 交互式解释器
```
python -m code  # 交互式控制台
python -m this  # Python 之禅
python -m antigravity  # 彩蛋
```

- 二维码生成
```
python -m qrcode "https://www.python.org"
```

#### 2.8 文档和帮助
- 查看模块文档
```
python -m pydoc json
python -m pydoc -p 8888  # 启动本地文档服务器
```

- 查看内置函数帮助
```
python -m builtins
```



<font size=6.5 color='red'>不管未来如何，坚持学习提升自我 ~~~</font>

