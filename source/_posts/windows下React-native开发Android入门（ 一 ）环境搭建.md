---
title: windows下React-native 环境搭建
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

公司决定试水react-native，mac审批还没下来，没办法，先用windows硬着头皮上吧。

参考文章：

[React Native 中文网官方文档](http://reactnative.cn/docs/0.31/getting-started.html#content)

[史上最全Windows版本搭建安装React Native环境配置](http://www.w2bc.com/article/103350)

##安装windows环境##

- Java Development Kit [JDK] 1.8+
- Python 2
- Node
- react-native-cli (React Native命令行工具)

先把jdk1.8+、Python2、node安装了，这三个不分先后，安装完毕记得添加环境变量，这里就不多说了。详细情况可以参照：

[搭建Java开发环境](http://www.cnblogs.com/yzg1/p/5354763.html)

[Node官网](https://nodejs.org/en/)

[python官网](https://www.python.org/)

你也可以使用`Chocolatey`,它是一个Windows上的包管理器，类似于linux上的yum和 apt-get。

安装Chocolatey：

	@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin

安装Python 2：

	choco install python2

安装Node：

	choco install nodejs.install

安装react-native-cli：

	npm install -g react-native-cli


## 搭建Android开发环境 ##

下载Android sdk tools ，它是一个Android sdk 管理工具。[点此下载](http://tools.android-studio.org/index.php/sdk)

下载安装完毕后，选择SDK Manager打开，下载以下package：

- 在Android 6.0 (Marshmallow)中勾选：
	- Google APIs
	- Intel x86 Atom System Image
	- Intel x86 Atom_64 System Image
	- Google APIs Intel x86 Atom_64 System Image

- 在SDK Tools中勾选
	- Android SDK Build-Tools 23.0.1。（**必须是这个版本**）

安装完毕后，添加ANDROID_HOME环境变量。至此，大功告成。


## 安装Android模拟器 ##

官方推荐Genymotion模拟器，我使用了下，确实很好用，并且它对个人是免费的（要注册）。[点此下载](https://www.genymotion.com/)

- genymotion需要依赖VirtualBox虚拟机，下载选项中提供了包含VirtualBox和不包含的选项，请按需选择）。

- 打开Genymotion。如果你还没有安装VirtualBox，则此时会提示你安装。

- 创建一个新模拟器并启动。

- 启动React Native应用后，可以按下F1来打开开发者菜单。

## run ##

初始化react-native项目

	react-native init <projectName>

启动项目（打开模拟器）

	cd <projectName>
	react-native run-android

这一步会启动服务并在模拟器上安装项目apk。如果在模拟器上看到你的app了，则表示成功了。

如果在你运行react-native run-android命令后，Packger可能不会自动运行，这时可以手动运行：

	react-native start

判断package运行成功与否可以通过访问：[http://localhost:8081/index.android.bundle?platform=android](http://localhost:8081/index.android.bundle?platform=android)。如果能打开页面则表示启动成功。


在模拟器上打开项目app，然后摇一摇手机（模拟器有摇一摇按钮），或者直接输入ctrl+m打开调试窗口，点击Dev Settings后，点击Debug server host & port for device,设置IP和端口，输入你的电脑的ip地址，并加上8081端口号。点击确定，重启app。这时你就能看到react-antive界面了。