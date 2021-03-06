---
layout:     post
title:      "appium环境搭建"
subtitle:   ""
date:       2016-09-12 12:35:00
author:     "shiciki"
header-img: "img/post-bg-2015.jpg"
tags:
    - appium
    - 自动化测试
    - android
    - python
---

appium网上教程也有很多了，但搭建的时候发现很多教程使用appium的版本较老，使得安装程序较为复杂，实际上并不需要那么复杂

#### 1.安装appium

appium可以直接去[官网](http://appium.io/index.html?lang=zh)下载安装,从网页到文档都有中文，非常便利

下载完成后，记得配置appium的环境变量

>\node_modules\\.bin  

如果以前没有安装过node，那么appium安装路径下已经集成了node，同样需要将路径加到环境变量path中

然后，可以在cmd中输入 $ appium-doctor 来检测是否安装成功，以及是否缺少运行环境
appium需要以下环境

+ Android_home
+ Java_home
+ Adb
+ Android
+ Emulator

别看那么多东西，其实只要安装JAVA和Android SDK,并配置好环境变量，这些就都OK了

#### 2、安装JAVA及Android SDK
这部分比较基础，已经安装并配置过的就可以跳过了

JAVA需要安装JDK
[java下载地址](http://www.java.com/zh_CN/download/manual.jsp)

安装完成后，需要将环境变量配置一下。

>JAVA_HOME 指向jdk安装的路径  
CALSS_PATH中添加 %JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;   
PATH中添加 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;  

配置完成后，在cmd中输入javac，看到命令提示信息就表示已经配置完成了

Android SDK可以在官网下载
[戳我下载](http://developer.android.com/sdk/index.html)
安装完成后，同样需要配置环境变量

>ANDROID_HOME  path of SDK  
PATH  %ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;  

装完这些后在cmd里再输入appium-doctor检查，是不是全部OK了？

![检查成功](/blog/img/post-160912.png)

到这一步，appium环境已经搭建完毕了，但appium还需要编写脚本运行。下面以python为例，调试appium的一个demo

#### 3、python
首先安装python，python3可以在{官网下载}(https://www.python.org/downloads/)安装
安装完成后，依然是老套路配置环境变量，把安装路径添加到PATH里
之后安装Appium-Python-Client
{下载地址}(https://pypi.python.org/pypi/Appium-Python-Client)
下载完成后，在cmd中 cd 到下载的路径
然后输入

> $ python setup.py install   

其他语言安装方法类似（大概

然后就挂上机器或者模拟器跑一个demo试试了:

    #coding:utf-8  
    from appium import webdriver  
    from time import sleep  
    
    desired_caps = {}  
    desired_caps['platformName'] = 'Android'  
    desired_caps['platformVersion'] = '4.4'  
    desired_caps['deviceName'] = 'Android Emulator'  
    desired_caps['app'] = 'Calculator.apk'  
    desired_caps['appPackage'] = 'com.android.calculator2'  
    desired_caps['appActivity'] = '.Calculator'  
     
    dr = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)  
    sleep(3)  
    
    dr.find_element_by_id('com.android.calculator2:id/digit9').click()  

如果运行成功，恭喜你appium已经配置好了
如果配置过程出现任何问题，也欢迎在下方留言~

