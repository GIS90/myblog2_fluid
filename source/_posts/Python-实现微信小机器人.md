---
title: Python实现微信小机器人
index_img: /img_index/index/20181019-001.jpg
desc: Python开发的一个微信小机器人，智能化微信机器人由此诞生
categories:
  - [Python]
tags: [Python, Python程序篇]
keywords: python, api, 图灵, 微信, 机器人, AI, 人工智能, 脚本, 程序
abbrlink: 12618
date: 2018-10-19 23:02:31
updated: 2018-10-19 23:02:31
---

{% label default@微信 %} {% label primary@Python实战 %} {% label success@图灵机器人 %} {% label info@自动化脚本 %}

### 背景
{% note primary %}
其实做这个微信小机器人的动机不纯，请勿见怪，也许我是一位假的程序猿😄。
{% endnote %}

>某天，我犯了我并不知道的错误，惹生气了女票，并且长达24h没有理我。我一看不行啊，总不能上班时间总一直抱着微信说话吧，而且最近微信小机器人貌似蛮火的，我也跟风，搞一搞。


<!--more-->
<hr />

### 技术架构

    python + itchat + 图灵机器人


### 准备材料

| id  |  name   |    Function    |
|:---:|:-------:|:--------------:|
|  1  | Mac电脑 |      开发      |
|  2  |  手机   |                |
|  3  | 微信号  |      微信      |
|  4  |  网络   | 电脑、手机使用 |

### 正文

首先，先实现微信的登录、接受消息、发送消息等基本功能。只有先实现了最基础的发消息功能，才可以做出自动回复的小机器人，接下来一步步去实现。文章中主要用到了[itchat](https://itchat.readthedocs.io/zh/latest/)这个包，官方有简单的case。

#### 登录
我自己封装了一个运行的主方法，加了一些是否开启特殊人处理、bug日志处理的参数，enableCmdQR开启二维码登录。登录之后发现我的PC端微信被挤掉了。。。
```
def run(is_unique=False, is_debug=False):
    """
    main method enter
    :param is_unique: is or not unique
    :param is_debug: is or not debug
    :return: None
    """
    init_work()
    global IS_UNIQUE, IS_DEBUG
    IS_UNIQUE = is_unique
    IS_DEBUG = is_debug
    itchat.auto_login(hotReload=True, enableCmdQR=2)
    itchat.run(True)
```
针对于***init_work()***这个方法，做了一个文件夹初始化的方法，用来记录日志、聊天信息。
```
def init_work():
    """
    initialize the chat user time file
    :return: None
    """
    user_file = get_default_refile()
    if os.path.exists(user_file) and os.path.isfile(user_file):
        return
    open(user_file, 'a').close()
    print 'init work of record user chat file is ok'


def get_default_refile():
    """
    default record file
    :return: file
    """
    cur_path = os.path.abspath(os.path.dirname(os.path.abspath(__file__)))
    user_file = cur_path + '/record_user.log'
    return user_file
```

#### 个人消息
注册各类型的消息，通过下列代码，微信可以接收、发送私聊的消息。itchat暂时支持接口TEXT, MAP, CARD, NOTE, SHARING, PICTURE这几种类型，因为用itacht是登录网页版微信进行操作，所以微信网页版支持这几种消息。
```
@itchat.msg_register([TEXT, MAP, CARD, NOTE, SHARING, PICTURE])
def handler_text_msg(msg):
    """
    auto reply messages [text, map, card, note, sharing] by friend or group
    :param msg: wx message (dict)
    :return: text message
    """
    print msg.get('Type')
    print json.dumps(msg)
```
参数msg是消息体，可以打印出来，是个标准的json，可以解析，举个最简单的例子，消息体自己慢慢看，我就说几种重要的。

![itchat_msg](itchat_msg.png)

```
 "ToUserName": 接受人微信ID,
 "FromUserName": 发送人微信ID,
"Content": 微信内容
"Type": 微信消息类型
```
发送消息
```
itchat.send(rely_msg_text, toUserName=form_user_name)
```
rely_msg_text：消息内容
toUserName：接受人ID，就是上面消息题解析的

#### 群消息
群消息也是一样，通过注册进行获取，但是获取全部的群消息太多了，我只选取了几个群进行消息监听。群消息体json在这里不做展示，几乎与chat消息体内容差不多，代码如下：
```
@itchat.msg_register([TEXT, SHARING, SYSTEM], isGroupChat=True)
def group_text_reply(msg):
    print json.dumps(msg)
    print '%s: %s' % (msg.get('ActualNickName'), msg.get('Text'))

    monitor_chats = [
                     # u'家族群',
                     u'宝龙山&amp;保康！.宝龙山&amp;保康',
                     u'媳妇私房钱'
                    ]
    chat_id_list = list()
    for chat in monitor_chats:
        if not chat:
            continue
```
群消息类型不像chat那样类型那么多，只有几种。
#### 注册图灵

- 登录&&注册
官网：http://www.tuling123.com/member/robot/index.jhtml
![itchat_tuling_login](itchat_tuling_login.png)
- 创建
![itchat_tuling_create](itchat_tuling_create.png)
- 设置
![itchat_tuling_set](itchat_tuling_set.png)
- apikey
![itchat_tuling_key](itchat_tuling_key.png)

#### 小机器人
注册好小机器人之后，只需要查看[api教程](https://www.kancloud.cn/turing/www-tuling123-com/718229)进行http请求即可，代码如下：
```

def reply_by_ai(msg):
    """
    auto reply by ai robot
    :param msg: message body by wx friend
    :return: send message
    """
    user_name = msg.get('FromUserName')
    msg_type = msg.get('Type')
    msg_text = msg.get('Text')
    API_ROBOT_URL = 'http://openapi.tuling123.com/openapi/api/v2'
    API_KEY = '65d96c7612e14a3ba8c6d43fa7a84111'
    USER_ID = '113972'

    payload = {
        "reqType": 0,
        "perception": {
            "inputText": {
                "text": msg_text
            }
        },
        "userInfo": {
            "apiKey": API_KEY,
            "userId": USER_ID
        }
    }
    payload = json.dumps(payload)
    headers = {"Content-Type": "application/json"}

    resp = requests.post(url=API_ROBOT_URL,
                         headers=headers,
                         data=payload)
    try:
        resp_json = resp.json()
        code = resp_json.get('intent').get('code')
        if resp.status_code == 200 and code >= 10000:
            result = resp_json.get('results')[0]
            rely_msg_text = result.get('values').get('text')
        else:
            rely_msg_text = "小6好像出问题了, 正在通知主人回来抢修"
    except:
        rely_msg_text = "小6没有找到答案😭😭😭, 尝试换个话题吧"
    finally:
        return rely_msg_text
```

#### 温馨提示

+ 微信消息不要连续发，会被腾讯禁止一段时间内不能发消息

### 总结

总的来说，实现起来并不难，喜欢记录的同学后续可以打算把消息存到自己的DB里去。没事就喜欢捣鼓捣鼓，只有真的去搞了，才能亲身体验其中的奥秘～代码带来的快乐。

### 学习资料

ITCHAT(py官方)：https://pypi.org/project/itchat/
ITCHAT(中文)：https://itchat.readthedocs.io/zh/latest/
图灵API：https://www.kancloud.cn/turing/www-tuling123-com/718229


### github

我把项目上传到了github，有喜欢的同学下载看看，可直接运行。

    微信小机器人：https://github.com/GIS90/itchatmy
