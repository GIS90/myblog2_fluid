---
title: 视频工具Ffmpeg的基础学习
index_img: /img_index/index/20191215-001.png
categories:
  - [工具集]
tags: [ffmpeg, 命令工具]
abbrlink: 37692
date: 2019-12-15 15:19:18
updated: 2019-12-17 15:19:18
desc: ffmpeg的基础学习，有关于视频、图片的处理工具
keywords: ffmpeg, 视频, 音频, 工具, 压缩, 格式转换
---

### 1、简介
{% note primary %}
大致的意思：FFmpeg是一款多媒体框架，能处理音频、视频等，不管什么格式的数据都可以进行处理，支持MacOS、Windows、Linux等各种系统。
{% endnote %}




{% label default@ffmpeg %} {% label primary@视频 %} {% label success@音频 %} {% label info@格式转换 %} {% label danger@压缩 %}


<!--more-->
<hr />

FFmpeg：<font size=6 color='red'>我是处理媒介数据方面的专家，我很牛逼，快来用我吧！！！</font>
我：<font size=6 color='blue'>好的，👌！</font>

![](article_ffmpeg.png)

### 2、背景

因为公司做了一个宣传片，把宣传片视频上传到第三方网站上，发现非会员会有广告。这还得了，如果客户没有会员，岂不是要等，为了解决这个问题，从一个程序员的角度来想办法，直接做个页面，把视频放进去，通过域名访问就可以解决无广告的问题。

周末花了一上午的时候搞完了，通过域名直接访问看，可以播放视频，但是发现视频有点卡，查看了一下视频大小：6分钟的视频500M，有点大啊，于是想压缩一下，上网搜索***在线 视频压缩***，TMD，要不就收费、要不就限制100M，身为程序员的我咋可能花钱去处理。于是，继续搜索***python 视频压缩***，看到了自己没有见过的***ffmpeg***命令，查了下资料，评价还不错，花了1小时搞定了***ffmpeg***环境，压缩 + 转格式的方式处理，最后视频110M，压缩了将近4/5，为了方便日后使用以及推荐给大家使用，写下本篇。

背景介绍完毕了，开始搞。

### 3、下载

安装需要下载2个文件：ffmpeg音视频处理工具，yasm是一款汇编器。yasm是安不安装都可以，既然ffmpeg里面都要求了就装上，省得日后麻烦。
文件下载地址：
yasm：http://yasm.tortall.net/Download.html
ffmpeg：http://ffmpeg.org/download.html

都是国外下载地址，如果翻不过去，本篇提供下载地址，百度云盘：

|  name  |                       url                       | password |
|:------:|:-----------------------------------------------:|:--------:|
|  yasm  | https://pan.baidu.com/s/14CEpqZvDhgyIcwjsINw4Gg |   ymw5   |
| ffmpeg | https://pan.baidu.com/s/1n65Ss2S7rtp5bYIFxKHeHQ |   j3oj   |

### 4、安装

#### 4.1、上传 && 解压

把yasm && ffmpeg文件下载到本地，需要上传到Linux服务器，在这里我采用的***scp***命令。

```
scp ~/Downloads/ffmpeg/* root@192.168.151.64:~
```

上传完之后，ssh连接服务器，找到上传的文件进行解压：

```
# ffmpeg
tar -jxvf ffmpeg-4.2.1.tar.bz2

# yasm
tar -zxvf yasm-1.3.0.tar.gz
```
看好了，在这里yasm用的参数是**z**，ffmpeg用的是**j**，因为压缩的格式不一样，所以参数是不一样的，不知道的请看下tar的用法。


#### 4.2、yasm安装

进入到yasm-1.3.0进行编译安装，因为是源码：
```
cd yasm-1.3.0/
./configure
make
make install
```
这个安装过程是没有任何问题的，等待安装完成即可。

#### 4.3、ffmpeg安装

进入ffmpeg-4.2.1文件夹进行编译安装。
```
./configure --enable-shared --prefix=/usr/loacl/ffmpeg
make
make install
```
编译过程有点长，请耐心等待，也可以做些别的事情。
安装成功后，***cd /usr/loacl/ffmpeg***进入安装目录，查看一下发现有bin、include,、ib、share这4个目录，
- bin：是ffmpeg主程序二进制目录
- include是C/C++头文件目录
- lib是编译好的库文件目录
- share是文档目录

#### 4.4、lib配置

> ld.so.conf

编辑lib包的配置文件，加入***/usr/local/ffmpeg/lib***内容（/usr/local/ffmpeg是ffmpeg的安装目录，根据个人不同安装目录修改）。
```
vim /etc/ld.so.conf
```
更新环境变量
```
ldconfig
```

> /etc/profile

打开/etc/profile文件，在文件末尾加入以下内容:
export PATH="/usr/local/ffmpeg/bin:$PATH"
```
vim /etc/profile
```
然后保存并运行：
```
source /etc/profile
```

#### 4.5、运行

如果上步配置正确了，那么运行ffmpeg命令就会出现下图所示，因为ffmpeg不是系统命令，也不再系统PATH里面，所以执行要写全路径+ffmpeg。

```
[root@localhost /usr/local/ffmpeg/bin]#./ffmpeg
ffmpeg version 4.2.1 Copyright (c) 2000-2019 the FFmpeg developers
  built with gcc 4.8.5 (GCC) 20150623 (Red Hat 4.8.5-39)
  configuration: --enable-shared --prefix=/usr/local/ffmpeg
  libavutil      56. 31.100 / 56. 31.100
  libavcodec     58. 54.100 / 58. 54.100
  libavformat    58. 29.100 / 58. 29.100
  libavdevice    58.  8.100 / 58.  8.100
  libavfilter     7. 57.100 /  7. 57.100
  libswscale      5.  5.100 /  5.  5.100
  libswresample   3.  5.100 /  3.  5.100
Hyper fast Audio and Video encoder
usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...

Use -h to get full help or, even better, run 'man ffmpeg'
```

#### 4.6、问题

```
libavdevice.so.57: cannot open shared object file: No such file or directory
```
如果出现了以上问题，那么就检查一下在lib配置配置那步，是否做对了；如果解决不了，欢迎留言咨询。


### 5、使用

总结一下本人暂时学到的一些基础使用方法，这个命令很强大，喜欢的可以去官网进行查看详细的使用方法。

#### 5.1、信息获取

> 获取媒体信息
```
/usr/local/ffmpeg/bin/ffmpeg -i file_name
```

```
[root@localhost ~]#/usr/local/ffmpeg/bin/ffmpeg -i elt.mp4
ffmpeg version 4.2.1 Copyright (c) 2000-2019 the FFmpeg developers
  built with gcc 4.8.5 (GCC) 20150623 (Red Hat 4.8.5-39)
  configuration: --enable-shared --prefix=/usr/local/ffmpeg
  libavutil      56. 31.100 / 56. 31.100
  libavcodec     58. 54.100 / 58. 54.100
  libavformat    58. 29.100 / 58. 29.100
  libavdevice    58.  8.100 / 58.  8.100
  libavfilter     7. 57.100 /  7. 57.100
  libswscale      5.  5.100 /  5.  5.100
  libswresample   3.  5.100 /  3.  5.100
Input #0, mov,mp4,m4a,3gp,3g2,mj2, from 'elt.mp4':
  Metadata:
    major_brand     : mp42
    minor_version   : 0
    compatible_brands: mp42mp41
    creation_time   : 2019-12-12T07:49:20.000000Z
  Duration: 00:06:22.93, start: 0.000000, bitrate: 10291 kb/s
    Stream #0:0(eng): Video: h264 (Main) (avc1 / 0x31637661), yuv420p, 1920x1080 [SAR 1:1 DAR 16:9], 9966 kb/s, 25 fps, 25 tbr, 25k tbn, 50 tbc (default)
    Metadata:
      creation_time   : 2019-12-12T07:49:20.000000Z
      handler_name    : ?Mainconcept Video Media Handler
      encoder         : AVC Coding
    Stream #0:1(eng): Audio: aac (LC) (mp4a / 0x6134706D), 48000 Hz, stereo, fltp, 317 kb/s (default)
    Metadata:
      creation_time   : 2019-12-12T07:49:20.000000Z
      handler_name    : #Mainconcept MP4 Sound Media Handler
At least one output file must be specified
```

音频、视频都可以，读取文件的相关信息。

> 查看支持格式
```
/usr/local/ffmpeg/bin/ffmpeg -formats
```

```
[root@localhost ~]#/usr/local/ffmpeg/bin/ffmpeg -formats | head -n 10
ffmpeg version 4.2.1 Copyright (c) 2000-2019 the FFmpeg developers
  built with gcc 4.8.5 (GCC) 20150623 (Red Hat 4.8.5-39)
  configuration: --enable-shared --prefix=/usr/local/ffmpeg
  libavutil      56. 31.100 / 56. 31.100
  libavcodec     58. 54.100 / 58. 54.100
  libavformat    58. 29.100 / 58. 29.100
  libavdevice    58.  8.100 / 58.  8.100
  libavfilter     7. 57.100 /  7. 57.100
  libswscale      5.  5.100 /  5.  5.100
  libswresample   3.  5.100 /  3.  5.100
File formats:
 D. = Demuxing supported
 .E = Muxing supported
 --
 D  3dostr          3DO STR
  E 3g2             3GP2 (3GPP2 file format)
  E 3gp             3GP (3GPP file format)
 D  4xm             4X Technologies
  E a64             a64 - video for Commodore 64
 D  aa              Audible AA format files
```


查看支持的音频、视频格式。

#### 5.2、格式转换

> 基础使用

```
/usr/local/ffmpeg/bin/ffmpeg -i elt.mp4 elt_output.avi

# 多格式输出
/usr/local/ffmpeg/bin/ffmpeg -i elt.mp4 elt_output.avi elt_output.wmv
```

![](ffmpeg_i_tf.png)

命令行中指定文件的输入和输出文件名就行了，这个方法同样适用于视频和音频文件，这个是常用的命令之一。

> 保留视频质量

```
/usr/local/ffmpeg/bin/ffmpeg -i elt.mp4 -qscale 0 elt_output.avi

```

通过-qscale 0参数来保留视频格式转换的视频质量。

#### 5.3、视频压缩

> 降低比特率

```
ffmpeg -i elt.mp4 -b:v 1000k -bufsize 1000k elt.mp4
```
通过降低比特率(使用 -b:a 或 -ab)来进行视频压缩，一些常用的比特率有: 96k, 112k, 128k, 160k, 192k, 256k, 320k.值越大，文件所需要的体积就越大。


### 6、学习参考

ffmpeg官网：http://ffmpeg.org/ffmpeg.html#Audio-Options
Linux上的ffmpeg完全使用指南：https://eyehere.net/2019/the-complete-guide-for-using-ffmpeg-in-linux/#basic-usage

### 7、结束语

ffmpeg的安装与基础使用先记录到这里吧，但是关于ffmpeg的探索不会停止，学习永无止境，日后有使用到其他命令会更新此博客。

<font size=6 color='red'>不会的东西，干它就完了！！！</font>
