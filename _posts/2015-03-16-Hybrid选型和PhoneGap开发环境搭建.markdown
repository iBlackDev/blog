---
layout: post
title:  "Hybrid选型和PhoneGap开发环境搭建"
date:   2015-03-16 11:29:47
categories: iOS
---

#### 简介

主流的Hybrid App现在有两种方案：

* 以PhoneGap为代表，使用HTML页面进行构建的App
* 以Titanium为代表，通过NodeJS驱动编译Native层的代码，从而产生近似于Native的效果
关于Titanium，在另外一篇文章里做尝试，今天纪录的是PhoneGap开发的环境搭建

#### WHY (为什么要这么做)

对于使用native和hybrid的哪一个能更好的开发，经常会有争执。不谈争执，只说为什么要选择hybrid

* 开发速度快。早期以Hybrid进行快速开发快速试错，甚至可以在其中采用ABTesting验证哪一种设计更好，当版本逐渐稳定，从Hybrid过度到Native可以说是现在最好的开发模式
* 快速发布。如果没有淘宝或者微信的推送更新功能，那还是考虑用Hybrid的增量更新功能，甚至有些活动页面可以直接访问线上站点，提高更新的效率，绕开了某些store的审核机制
* 大规模团队协作。以模块进行切分，最后合并不需要做整体的build优化了开发的效率

所以选型上就直接选择了hybrid毕竟开发人员少的情况下这是最好的方案，比较了国内的AppCan没有犹豫的选择了PhoneGap，后者开发的时间长，文档和各种辅助工具齐全。


#### HOWTO (如何去做)

PhoneGap号称已经免费了，但是考虑到它有收费的不良记录还是采用了它的开源版本cordova，cordova 是基于 apache 协议进行开发的。

在开发过程中采用了基于nodejs的The Command-Line Interface

1. 安装nodejs
2. 安装coordova模块

	```
	sudo npm install -g cordova
    ```
	这个安装过程需要花费很长的时间，推荐采用淘宝的npm镜像。

3. 安装android开发环境并且配置环境变量，在Terminal里面输入android看看有没有android的版本管理器出来就说明配置有没有做好，关于如何配置环境变量搜素一下，mac推荐看这里
4. 安装ant，cordova采用ant来做的持续集成，需要配置ant环境，搜素一下，mac的看这里
5. 创建HelloWorld执行命令

	```
	cordova create hello com.example.hello HelloWorld
	```
	这个过程异常的艰难，希望你有个好网络

6. 配置开发平台，进入hello目录，执行
	
	```
	cordova platform add android
	```
7. 编译
	
	```
	cordova build
	```
8. 安装，非常不喜欢虚拟机，所以直接插上android运行

	
	```
	cordova run android
	```
	如果希望启动虚拟机
	
	
	```
	cordova emulate android
	```
	
	然后一个很傻的，没有什么功能的应用就装在手机上了

9. 进一步开发，用Android Studio导入工程，在\hello\www目录下就是html开发内容，hybrid的开发就在这里做

在这个阶段中对环境变量的修改

```
export PATH="$PATH:/Users/xxx/android-sdk-macosx/platform-tools"
export PATH="$PATH:/Users/xxx/android-sdk-macosx/tools"
export PATH="$PATH:/Users/xxx/android-sdk-macosx"
export PATH="$PATH:/Users/xxx/apache-ant-1.9.4/bin"
export JAVA_HOME=`/usr/libexec/java_home`
```

#### SUPPLEMENT（补充）

mobile web ui的选型，列举一下现在流行的一些ui库作为选型的标的

* ionic
* famo

famo与angularjs进行了深度的整合，但是考虑到ionic对cordova的封装，准备用ionic


1.安装ionic cli

```
$ npm install -g cordova ionic
```

2.创建项目

```
$ ionic start myApp tabs
```

这个步骤非常的耗费时间，网络啊网络


```
➜  Project  ionic start bee-app tabs
Creating Ionic app in folder /Users/xxx/myApp based on tabs project

The directory /Users/xxx/myApp already exists.
Would you like to overwrite the directory with this new project?
(yes/no): yes

Downloading: https://github.com/driftyco/ionic-app-base/archive/master.zip
[=============================]  100%  0.0s

Downloading: https://github.com/driftyco/ionic-starter-tabs/archive/master.zip
[=============================]  100%  0.0s

Update config.xml
Initializing cordova project
Fetching plugin "org.apache.cordova.device" via plugin registry
Fetching plugin "org.apache.cordova.console" via plugin registry
Fetching plugin "com.ionic.keyboard" via plugin registry

Your Ionic project is ready to go! Some quick tips:

 * cd into your project: $ cd myApp

 * Setup this project to use Sass: ionic setup sass

 * Develop in the browser with live reload: ionic serve

 * Add a platform (ios or Android): ionic platform add ios [android]
   Note: iOS development requires OS X currently
   See the Android Platform Guide for full Android installation instructions:
   https://cordova.apache.org/docs/en/edge/guide_platforms_android_index.md.html

 * Build your app: ionic build <PLATFORM>

 * Simulate your app: ionic emulate <PLATFORM>

 * Run your app on a device: ionic run <PLATFORM>

 * Package an app using Ionic package service: ionic package <MODE> <PLATFORM>

For more help use ionic --help or visit the Ionic docs: http://ionicframework.com/docs

+---------------------------------------------------------+
+ New Ionic Updates for February 2015
+
+ The View App just landed. Preview your apps on any device
+ http://view.ionic.io
+
+ Add ngCordova to your project for easy device API access
+ bower install ngCordova
+
+ Generate splash screens and icons with ionic resource
+ http://ionicframework.com/blog/automating-icons-and-splash-screens/
+
+---------------------------------------------------------+

Create an ionic.io account to use the Ionic View app and other features?
(Y/n): Y
```

3.注册ionic

在ionic start的最后询问是否Create an ionic.io account to use the Ionic View app and other features?，选择Y，进行帐号注册

4.更新项目

```
$ ionic login 
$ ionic upload
```

5.运行项目

```
$ cd myApp
$ ionic platform add android
$ ionic build android
$ ionic run android
```

#### REFERENCE (参考)

* [$JAVA_HOME环境变量在Mac OS X中设置的问题](http://www.micmiu.com/lang/java/set-javahome-on-mac-os-x/)
* [Mac OS X，下载并安装ant](http://blog.csdn.net/crazybigfish/article/details/18215439)
* [Cordova](http://cordova.apache.org/)
* [The Command-Line Interface](http://cordova.apache.org/docs/en/4.0.0//guide_cli_index.md.html#The%20Command-Line%20Interface)
* [淘宝 NPM 镜像](http://segmentfault.com/a/1190000000471219)
* [性能、UX、跨平台：移动Web应用UI框架大比拼](http://www.csdn.net/article/2014-12-22/2823241-hybrid-app-ui-frameworks/2)


文章转自[leewind](http://segmentfault.com/blog/leewind/1190000002560220)
