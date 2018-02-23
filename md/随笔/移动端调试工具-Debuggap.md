title: 移动端调试工具-Debuggap
tags: [工具]
date: 2016-03-16 14:06:17
description: 移动端调试工具,Debuggap

---

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

随着移动互联网的迅速崛起，开发移动应用程序越来越多，但如果在移动端开发应用程序需要调试时，额… 仿佛又回到了IE时代，最方便也只能到处 alert 来调试。目前已经有一款产品可以做到这一点，比如phonegap，但是phonegap的调试问题非常麻烦，不能真正做到有效提高效率。下面将介绍debug工具。这是一款神器，它简单易用的同时又不影响它的强大，它能够：

<!-- more -->

+ 不需要安装即可运行在Windows、Linux和Mac平台上
+ 支持各种平台(Android,IOS,WebOS,BlackBerry,Firefox OS,Windows Phone等等)
+ 支持所有HTML5框架(比如phonegap)和浏览器
+ 支持快速查看元素节点树
+ 可以同一时间调试多个设备
+ 支持Android设备单步调试,观察变量等等


话不多说，动起来的吧。

### 1.下载解压
首先到[官网](http://www.debuggap.com/)根据自己的色板平台下载相应Debuggap,下载完毕后解压即可，无需安装。解压后，目录结构是这样的，其中DebugGap.exe是运行程序，双击即可运行；而client文件夹下存放的是DebugGap.js。
```bash
2016/01/25  15:50    <DIR>          .
2016/01/25  15:50    <DIR>          ..
2015/05/26  22:04    <DIR>          client
2015/04/01  22:05        39,340,032 DebugGap.exe
2015/03/29  15:39           860,672 ffmpegsumo.dll
2015/03/29  15:39         9,956,864 icudt.dll
2015/03/29  15:39           102,400 libEGL.dll
2015/03/29  15:39           873,984 libGLESv2.dll
2015/03/29  15:39         4,001,552 nw.pak
2015/03/29  15:39         4,207,616 nwsnapshot.exe
2015/05/26  22:02               231 package.json
2015/03/29  15:39               463 readme.txt
2016/01/26  13:48    <DIR>          source
               9 个文件     59,343,814 字节
               4 个目录 288,704,741,376 可用字节
```
### 2.使用
使用分为三部分：
+ 在项目中引入client文件夹下的DebugGap.js文件
+ 配置客户端
+ 启动debuggap程序

全过程示范：
第一步：新建一个测试页面。为了使我们手机能访问到，我们将测试页面放入xamp搭建的本地服务器中，并在页面中通过`<script src="debuggap.js" type="text/javascript"></script>`引入debuggap.js。（为了引用方便我已将debuggap.js拷到与测试页面同一个文件夹下）
第二步：使用手机访问页面，会发现页面上多了个蓝色按钮，点击后进入 config，配置地址为电脑ip地址和自定义的端口（出于偷懒，下面所有的图都是从[think2011](https://cnodejs.org/topic/54ff176fc1749396754897e5)拷的）
![](http://images2015.cnblogs.com/blog/733894/201603/733894-20160316132105209-1710430782.gif)


第三步：在电脑上运行 DebugGap.app，接着输入本机IP地址 和 自定义的端口。
![](http://images2015.cnblogs.com/blog/733894/201603/733894-20160316132130818-29108219.png)


点击链接，即可看到以下界面
![](http://images2015.cnblogs.com/blog/733894/201603/733894-20160316132144834-1036741627.png)



点击其中一个进入即可调试
![](http://images2015.cnblogs.com/blog/733894/201603/733894-20160316132210037-1723401607.png)

如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！

