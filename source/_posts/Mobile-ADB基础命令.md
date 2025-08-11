---
title: ADB基础命令
index_img: /img_index/index/20200927-001.jpeg
categories:
  - [移动端]
tags: [ADB]
abbrlink: 61770
date: 2020-09-27 18:23:34
updated: 2020-09-27 18:23:34
desc: ADB基础命令
keywords: ADB
---

**ADB**：Android Debug Bridge

<!--more-->

<hr />

搞了几天的ADB命令，记录一下，都是一些比较基础的命令。

环境：MacOs

#### 1、安装

```
brew install android-platform-tools
```

#### 2、服务

```
# 启动adb服务
adb start-server

# 结束adb服务
adb kill-server
```
指定端口服务：adb -P **port** start-server，这个port与设备tcpip开放的端口对应。

#### 3、连接

```
# 连接（one）-数据线
adb devices

# 连接/断开-IP
adb connect/disconnect 设备IP
```

#### 4、设备状态

```
adb get-state
```

- device：设备正常连接
- offline：连接出现异常，设备无响应
- unknown：没有连接设备

#### 5、重启设备

```
adb -s 设备名称 reboot
```
重启到recovery/bootloader模式：adb reboot recovery/bootloader。

#### 6、APP包

> 查看设备包

```
adb shell pm list packages
```
* 不加参数，默认：所有应用
* -f：显示应用关联的 apk 文件
* -d：只显示 disabled 的应用
* -e：只显示 enabled 的应用
* -s：只显示系统应用
* -3：只显示第三方应用
* -i：显示应用的installer
* -u：包含已卸载应用

> 获取当前窗口APP包名

```
# 第一种
adb shell dumpsys window | grep mCurrentFocus 
# 第二种
adb shell dumpsys activity activities | grep mResumedActivity
```
> 启动包

```
adb shell monkey -p com.ss.android.ugc.aweme -v 1
```
* 1: 代表事件次数
* -v: 日志级别，-v/-v -v/-v -v -v

包名：<a href="/articles/14917/" target="_blank" class="block_project_a">ADB设备包列表</a>。

> 安装/卸载应用程序

```
# 安装
adb install -r 安装包a.apk
# 卸载
adb uninstall com.ss.android.ugc.aweme
```
安装：
-r：允许覆盖安装
-s：将应用安装到 sdcard
-d：允许降级覆盖安装

卸载： -k：参数可选，表示卸载应用但保留数据和缓存目录

> 查看应用详细信息

```
adb shell dumpsys package <packagename>
```
输出中包含很多信息，包括 Activity Resolver Table、Registered ContentProviders、包名、userId、安装后的文件资源代码等路径、版本信息、权限信息和授予状态、签名版本信息等。

> 查看应用安装路径

```
adb shell pm path <PACKAGE>
```

> 清除应用数据与缓存

```
adb shell pm clear 包名
```

> 强制停止应用

```
adb shell am force-stop 包名（com.ss.android.ugc.aweme）
```
#### 7、查看设备信息

> 型号

```
adb shell getprop ro.product.model
```

> 电池状况

```
adb shell dumpsys battery
```

> 屏幕分辨率

```
adb shell wm size

# 修改
adb shell wm size 480x1024 

# 恢复
adb shell wm size reset
```

> 屏幕密度

```
adb shell wm density
```

> 屏幕参数

```
adb shell dumpsys window displays

# 显示范围
adb shell wm overscan 0,0,0,200 

# 恢复
adb shell wm overscan reset
```

> android_id

```
adb shell settings get secure android_id
```

> CPU

```
adb shell cat /proc/cpuinfo
```
与Linux查看实施cpu一致。

> 内存

```
adb shell cat /proc/meminfo
```
同上。

#### 8、设备shell

```
adb shell
```
ls, cd, rm, mkdir, touch, pwd, cp, mv, ifconfig, netstat, ping, ps, top等，进入adb shell即可执行，与linux相似。

#### 9、日志

```
# 查看日志
adb logcat

# 清除日志
adb logcat -c
```

#### 10、操作

> 点击

```
adb shell input tap 300 1000
```
x坐标 y坐标。

> 滑动

```
adb shell input swipe 540 1300 540 500 100
```
起始点x坐标 起始点y坐标 结束点x坐标 结束点y坐标。

> 输入文本
```
adb shell input text hello
```
焦点处于某文本框。

> 输入系统keycode

```
adb shell input keyevent XXXX
```
keycode具参考：<a href="/articles/11756/" target="_blank" class="block_project_a">ADB模拟按键代码</a>。

#### 11、复制文件

```
# 复制设备里的文件到电脑
adb pull <设备里的文件路径> [电脑上的目录]

# 复制电脑里的文件到设备
adb push <电脑上的文件路径> <设备里的目录>
```

#### 12、关闭USB调试模式

```
adb shell settings put global adb_enabled 0
```

#### 13、实用工具

> 截图

```
# 新
adb exec-out screencap -p > sc.png 

# 旧
adb shell screencap -p /sdcard/sc.png adb pull /sdcard/sc.png
```

> 录制屏幕

```
adb shell screenrecord /sdcard/filename.mp4 adb pull /sdcard/filename.mp4
```


{% raw %}
<div class="post_cus_note">其他</div>
{% endraw %}

#### 14、Connection refused

无线连接失败，执行一下命令：
```
adb tcpip 5555
```

#### 15、Windows过滤关键字

findstr


<font size=6.5 color='red'>持续更新中。。。。。。</font>













