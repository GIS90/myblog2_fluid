---
title: Python爬虫利器之requests(上篇)
index_img: /img_index/index/article_python_crawler.jpg
categories:
  - [Python]
tags: [Python, Python包, 爬虫系列]
abbrlink: 34296
date: 2019-04-04 19:01:28
updated: 2019-04-04 19:01:28
desc: Python爬虫利器之requests介绍
keywords: Python, Packages, 爬虫, BeautifulSoup, requests
---

{% label default@Python包 %} {% label primary@requests %} {% label success@爬虫教程系列 %} 

### 1、背景

身为一位程序员，没事的时候爬爬糗百笑话、淘宝女郎、小片片。。。。。。有点邪恶了。反正关于<font color='red' size=6.5>爬虫</font>，相信很多人都不陌生，本人没事的时候也写写爬虫的小项目，写过***天气报警、糗百笑话、美女图片。。。。。。***，后续会在博客中挑选个例子写出来。

<!--more-->



关于数据采集，用Python去写爬虫程序，主要涉及2个主要的Package：***Requests && BeautifulSoup***。

|     包名      | 功能说明             |
|:-------------:|:--------------------: |
|   Requests    | HTTP获取源数据       |
| BeautifulSoup | 解析 && 获取目标数据 |

关于这2个爬虫利器分为上下篇进行讲解，本篇先介绍***Requests***，主要功能是进行数据的获取。

<hr />


### 2、简介

官方原话：
```
The requests library is the de facto standard for making HTTP requests in Python.
It abstracts the complexities of making requests behind a beautiful,
simple API so that you can focus on interacting with services and
consuming data in your application.
```
简要内容：**Requests**是一个很实用的***Python HTTP***客户端库，主要用于**API**请求。

### 3、安装

```
pip install requests
```
如果不指定版本，会自动安装最新的版本，有不熟悉***pip***的请自行进行恶补。

### 4、源码说明

```
def request(method, url, **kwargs):
    """Constructs and sends a :class:`Request <Request>`.

    :param method: method for the new :class:`Request` object.
    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary or bytes to be sent in the query string for the :class:`Request`.
    :param data: (optional) Dictionary, bytes, or file-like object to send in the body of the :class:`Request`.
    :param json: (optional) json data to send in the body of the :class:`Request`.
    :param headers: (optional) Dictionary of HTTP Headers to send with the :class:`Request`.
    :param cookies: (optional) Dict or CookieJar object to send with the :class:`Request`.
    :param files: (optional) Dictionary of ``'name': file-like-objects`` (or ``{'name': ('filename', fileobj)}``) for multipart encoding upload.
    :param auth: (optional) Auth tuple to enable Basic/Digest/Custom HTTP Auth.
    :param timeout: (optional) How long to wait for the server to send data
        before giving up, as a float, or a (`connect timeout, read timeout
        <user/advanced.html#timeouts>`_) tuple.
    :type timeout: float or tuple
    :param allow_redirects: (optional) Boolean. Set to True if POST/PUT/DELETE redirect following is allowed.
    :type allow_redirects: bool
    :param proxies: (optional) Dictionary mapping protocol to the URL of the proxy.
    :param verify: (optional) if ``True``, the SSL cert will be verified. A CA_BUNDLE path can also be provided.
    :param stream: (optional) if ``False``, the response content will be immediately downloaded.
    :param cert: (optional) if String, path to ssl client cert file (.pem). If Tuple, ('cert', 'key') pair.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    
    Usage::
    
      >>> import requests
      >>> req = requests.request('GET', 'http://httpbin.org/get')
      <Response [200]>
    """
```

以上代码是Requests包中运用方法的核心，写代码多看看<font color='red' size=6.5>源码</font>，提高代码的质量，很重要。
上面的方法是关于HTTP请求的核心方法，挑重点说，方法的参数很多。其中***method***代表HTTP请求的方式，GET、POST等等；***url***代表请求的地址；***kwargs***代表不限参数，但是在注释中罗列了一些重要的参数，查看以你发现，其中这些参数都是关于***HTTP协议***用到的数据。参数的具体请自动查看源码，下面也会针对于常用的参数进行demo举例。

### 5、使用教程

HTTP协议请求方式主要有：GET、POST、HEAD、OPTIONS、PUT、PATCH、DELETE、TRACE和CONNECT方法。虽然方法很多，但是工作中常用的只有<font size=5 color='red'>GET</font>与<font size=5 color='blue'>POST</font>。
本人主要对***Requests***进行介绍，关于HTTP协议的相关知识这个不做过多讲解。

#### 5.1、无参

```
import requests


url = "https://gis90.github.io/"

# GET
response = requests.get(url)

# POST
response = requests.post(url)
```
    最简单的HTTP请求。

#### 5.2、有参

```
import json
import requests


url = "https://gis90.github.io/"
payload = {"key": "value"}

# GET
response = requests.get(url=url, params=payload)

# POST
response = requests.post(url=url, data=payload)
```

GET方法的params参数实际就是url中的参数，使用GET方法可以把参数直接封装在url中，也可以把参数封装起来传递到request方法中。
    POST方式的参数就是form表单提交的参数。

#### 5.3、json数据

```
import json
import requests

# headers中添加上content-type这个参数，指定为json格式
headers = {'Content-Type': 'application/json'}
# post的时候，将data字典形式的参数用json包转换成json格式。
payload = {"key": "value"}
payload = json.dumps(payload)
url = "https://gis90.github.io/"

# POST
response = requests.post(url=url, headers=headers, data=payload)
```

POST可传递json数据，需要在headers里指定数据的类型。

#### 5.4、文件

```
import requests

files = {'file': open('report.xls', 'rb')}
url = "https://gis90.github.io/"

r = requests.post(url, files=files)
```

把文件读取到内存中，在以流的形式传递出去。

#### 5.5、请求头

```
import json
import requests


url = "https://gis90.github.io/"
payload = {"key": "value"}
headers = {"user-agent": "my-app/0.0.1"}

# GET
response = requests.get(url=url,
                        params=payload,
                        headers=headers)

# POST
response = requests.post(url=url,
                         data=json.dumps(payload)
                         headers=headers)
```
所有的header值必须是string、bytestring或者unicode。

#### 5.6、Cookie

```
import requests


url = "https://gis90.github.io/"
# 第一种
cookies_1 = dict(key_1='value_1')
response = requests.get(url, cookies=cookies_1)
# 第二种
cookies_2 = requests.cookies.RequestsCookieJar()
cookies_2 = jar.set('key_2', 'value_2', domain=url, path='/cookies')
r = requests.get(url, cookies=cookies_2)

# 查看cookies信息以及类型
print response.cookies, type(response.cookies)
```
demo中给出了2种set cookies的方式，用第一种的较多，其中输出的cookies是一个***requests.cookies.RequestsCookieJar对象***。

#### 5.7、 timeout

```
import requests


url = "https://gis90.github.io/"
response = requests.get(url, timeout=5)
```
请求时间，可选参数。以设定参数timeout秒数时间之后停止等待响应，超过这个实际会停止对服务端的请求，如果是一般的restful api接口也就是几百毫秒的响应时间，如果是大数据量可以设置长一点，默认是无限制。
建议最好，设定一个值，这样请求timeout之后，也会报异常，对异常进行捕捉可以对方法进行监控报警，知道程序是否出现异常状况，时间根据对方的接口而定，建议60 ～ 120，单位：秒。

#### 5.8、allow_redirects

```
import requests


url = "https://gis90.github.io/"
response = requests.get(url, allow_redirects=False)
```
参数之一，允许请求重定向，默认值是True，所以这个参数只有有不允许重定向需求的时候采用得到。
**HTTP重定向**：服务器无法处理本次发送过来的request请求，服务器会返回一个新的url让客户端可以进行跳转，客户端会自动访问该url地址，但是在客户端是无法分辨是否重定向了， 重定向的状态码是**3XX**，关于状态码下面有粗略的介绍。

#### 5.9、 RESPONSE

```
import requests


data = {'key': 'value'}
response = requests.get(url='https://gis90.github.io/', data=data)

# 请求地址
print response.url
# 响应状态码
print response.status_code
# 响应头
print response.headers
# 文本编码
print response.encoding
# 字符串格式响应内容
print response.text
# 二进制格式响应内容
print response.content
# json格式响应内容
print response.json()
# 原始套接字响应内容
print response.raw
# cookie信息
print response.cookies
```
    
总结一下需要说明得点。
    
> 状态码
    
    {% raw %}
    <table>
      <tr>
        <th style="text-align:center;">类型</th>
        <th style="text-align:center;">描述</th>
      </tr>
      <tr>
        <td style="text-align:center;">1**</td>
        <td style="text-align:center;">信息，服务器收到请求，需要请求者继续执行操作</td>
      </tr>
      <tr>
        <td style="text-align:center;">2**</td>
        <td style="text-align:center;">成功，操作被成功接收并处理</td>
      </tr>
      <tr>
        <td style="text-align:center;">3**</td>
        <td style="text-align:center;">重定向，需要进一步的操作以完成请求</td>
      </tr>
      <tr>
        <td style="text-align:center;">4**</td>
        <td style="text-align:center;">客户端错误，请求包含语法错误或无法完成请求</td>
      </tr>
      <tr>
        <td style="text-align:center;">5**</td>
        <td style="text-align:center;">服务器错误，服务器在处理请求的过程中发生了错误</td>
      </tr>
    </table>
    {% endraw %}
    
> 响应头
    
    返回来的headers是一个***requests.structures.CaseInsensitiveDict对象***，但是直接可以用dict字典的方式进行数据获取。
    
> text与content
    
    text与content都是返回网页的内容，唯一的区别：content是字节码，而text的内容编码是字符串。
    
> cookies
    
    cookies在上面有详细说明。

### 6、学习参考

Requests官方：https://realpython.com/python-requests/
快速上手（中文版）：https://cn.python-requests.org/zh_CN/latest/user/quickstart.html
快速上手（高阶版）：https://cn.python-requests.org//zh_CN/latest/

### 7、特别技巧
{% note danger %}

Postman工具，相信做开发没有几个人不知道，它的最大用处就是用来实现各种HTTP请求。

今天在这里教大家一个技巧，如果使用requests包去模拟HTTP请求失败的话，就用Postman工具去请求，毕竟是可视化的工具，参数、cookies、headers等等比较好设置，只要在Postman上成功实现了请求，使用下图的功能，就可以查看关于请求的相关代码，很方便。

Postman下载地址：https://www.getpostman.com/
{% endnote %}

![](postman.png)