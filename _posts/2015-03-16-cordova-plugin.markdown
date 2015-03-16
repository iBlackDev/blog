---
layout: post
title:  "Cordova Plugin"
date:   2015-03-16 11:29:47
categories: jekyll update
---

## Cordova Plugin

### 简介
A plugin is a package of injected code that allows the Cordova webview within which the app renders to communicate with the native platform on which it runs. Plugins provide access to device and platform functionality that is ordinarily unavailable to web-based apps. All the main Cordova API features are implemented as plugins, and many others are available that enable features such as bar code scanners, NFC communication, or to tailor calendar interfaces. There is a registry of available plugins.

### 安装和使用

#### 安装
    $ cordova plugin add xxx
注：xxx可以是plugin ID或者plugin的git镜像地址，比如系统状态栏的plugin ID是org.apache.cordova.statusbar
    在git上面的镜像地址是https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git

获取plugin：

* [plugins.cordova.io](http://plugins.cordova.io)
* [github.com](https://github.com/search?utf8=%E2%9C%93&q=cordova+plugin&type=Repositories&ref=searchresults)
* [ngcordova](http://ngcordova.com)
* [Telerik](http://plugins.telerik.com/)

#### 删除
    $ cordova plugin remove xxx


#### 自定义plugin
cordova项目目录结构:
    
    ├── platforms
    |    ├── android
    |    ├── ios
	|    ├── wp7
	|    └── ...
	├── plugins
	|    ├── org.apache.cordova.statusbar
	|    └── ...
	├── config.xml
	└── www
	
`platforms`是项目支持的平台，可以通过*$ cordova platform add ios*添加新的平台，`plugins`是安装的插件目录，我们从cordova下载的插件和我们自己扩展的插件最终都是要添加在plugins目录中，`config.xml`是项目的配置信息文件，`www`是web工程目录。

扩展cordova plugin需要按照固定的目录结构创建，最终plugin源码对应的目录为：
	
	StatusBar
	├── src
	|    ├── android
	|    |    └── StatusBar.java
	|    ├── ios
	|    |    └── CDVStatusBar.h
	|    |    └── CDVStatusBar.m
	|    └── ...
	├── www
	|    └── StatusBar.js
	└── plugin.xml
	
`src`对应的是不同平台实现plugin的源码，`www`存放javascript，plugin.xml是plugin的配置文件。
plugin.xml的配置信息如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<plugin xmlns="http://apache.org/cordova/ns/plugins/1.0"
           id="com.sf.cordova.statusbar"
      version="0.3.0">

    <name>StatusBar</name>
    <description>扩展的cordova插件</description>
    <license>Apache 2.0</license>
    <keywords>cordova,statusbar</keywords>

    <js-module src="www/StatusBar.js" name="StatusBar">
        <clobbers target="cordova.plugins.StatusBar"/>
    </js-module>

    <!-- android -->
    <platform name="android">
        <config-file target="res/xml/config.xml" parent="/*">
            <feature name="StatusBar">
                <param name="android-package" value="com.sf.cordova.StatusBar"/>
            </feature>
        </config-file>

        <source-file src="src/android/StatusBar.java" target-dir="src/com/sf/cordova/statusbar" />
    </platform>

    <!-- ios -->
    <platform name="ios">
        <config-file target="config.xml" parent="/*">
            <feature name="StatusBar">
                <param name="ios-package" value="CDVStatusBar"/>
            </feature>
        </config-file>
        <header-file src="src/ios/CDVStatusBar.h" />
	    <source-file src="src/ios/CDVStatusBar.m" />
 </platform>
</plugin>
```

* `id`:cordova plugin ID
* `name`:plugin 名称
* `js-module`:javascript文件配置信息
* `platform`:插件支持的平台

StatusBar.js的内容为：

```
var exec = require('cordova/exec');

exports.hideStatusBar = function(success, error) {
    exec(success, error, "StatusBar", "hideStatusBar", []);
};
```
第一个`hideStatusBar`为暴露给JS的接口，第二个`hideStatusBar`是暴露给平台的接口，各平台需要实现各自的逻辑。
其中IOS平台需要继承CDVPlugin，并实现StatusBar.js中配置的接口，CDVStatusBar.m内容如下

```
#import "CDVStatusBar.h"
#import <objc/runtime.h>
#import <Cordova/CDVViewController.h>

@implementation CDVStatusBar

- (void)hideStatusBar:(CDVInvokedUrlCommand*)command
{
    CDVPluginResult* pluginResult = nil;
    NSString* echo = @"隐藏StatusBar";//这个地方是处理的结果，也就是native 的要返回给前端界面的参数，         
    NSLog(@"echo==%@",echo);
    if (echo != nil && [echo length] > 0) {
        pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_OK messageAsString:echo];
    } else {
        pluginResult = [CDVPluginResult resultWithStatus:CDVCommandStatus_ERROR];
    }
    
    [self.commandDelegate sendPluginResult:pluginResult callbackId:command.callbackId];
}

@end
```
Android平台的StatusBar.java需要继承CordovaPlugin类，重写execute(),使用 action 来判断我们在 javascript 中调用的方法名，成功的话调用 callbackContext.success(message)，失败调用 callbackContext.error(message) 方法，分别对应 javascript 文件中的 success 和 error 回调函数。
StatusBar.java的内容为：

```
package com.sf.cordova.statusbar;

public class StatusBar extends CordovaPlugin {

    public boolean execute(String action, JSONArray args, CallbackContext callbackContext) 
            throws JSONException {
        Activity activity = this.cordova.getActivity();
        if (action.equals("hideStatusBar")) {
            Intent i = activity.getIntent();
            if (i.hasExtra(Intent.EXTRA_TEXT)) {
                callbackContext.success(i.getStringExtra(Intent.EXTRA_TEXT));
            } else {
                callbackContext.error("");
            }
            return true;
        }
        return false;
    }
}
```

完成插件的编写，通过cordova的命令添加plugin：
	
	cordova plugin add StatusBar
	
项目plugins目录下会生成自定义的插件。

#### 使用Plugin

在前端页面的JS中调用插件代码：

	var statusbar = cordova.require('com.sf.cordova.statusbar.StatusBar');

    statusbar.hideStatusBar(function(message) {
        // alert(message);
    }, function(message) {
        // alert(message);
    });


#### 发布Plugin

开发好的plugin，如果想分享到Cordova registry，可以使用cordova提供的插件管理工具plugman发布上去，步骤如下：

	$ plugman adduser # that is if you don't have an account yet
	$ plugman publish /path/to/your/plugin

























