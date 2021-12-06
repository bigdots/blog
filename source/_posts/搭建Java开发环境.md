---
title: 搭建Java开发环境
categories: [js]
date: 2016-03-28 11:25:02
description: 搭建Java开发环境
---

Java 开发环境的搭建相对其他语言可能有些复杂，因为Java 本身提供了很多机制，从而方便学习。Java的开发环境可以用JDK 来代表，本章主要记录如何下载、安装和配置JDK。
<!-- more -->
## 安装配置JDK
1. 下载JDK
JDK是java的SDK（软件开发工具包Software Development Kit）。它是Sun 公司提供的一种免费的Java 软件开发工具包，里面包含了很多用于Java 程序开发的工具，最常用的是编译和运行工具。可以在[JDK官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载。

2. 安装JDK
下载JDK 后，双击下载的EXE 文件，即可开始安装JDK。首先是弹出许可证协议窗口，其中给出了Sun 公司的一些开发协议，单击其中的“接受”按钮。在窗口中可以选择要安装的Java 组件和JDK 文件的安装路径。Java的所有组件默认安装在C盘，也可以自定义安装路径。 。一直单击“下一步”按钮即可，稍后，单击窗口中的“完成”按钮就正式完成了JDK 的安装。

3. 配置JDK
这里才是重头戏，下载和安装JDK 只是完成Java 开发环境搭建的前半部分。配置JDK 的目的是能够在命令提示符中运行JDK 中的命令，例如编译和运行。
配置步骤：
 + 在“计算机”桌面图标上，单击鼠标右键，在弹出菜单中选择“属性”→“高级系统设置”→“环境变量”命令，弹出“环境变量”窗口，在该窗口中就可以进行环境变量的设置
 + 单击“系统变量”选项组中的“新建”按钮，新建环境变量，变量名为JAVA_HOME，变量值为JDK安装路径,默认为C:\Program Files (x86)\Java\jdk1.8.0_77;
 + PATH中新添%JAVA_HOME%in;

在完成了上诉环境配置后，打开cmd命令行，输入java -version,不报错则表示成功。

## 安装intelli idea
1. 下载安装intelli idea
官网下载:[http://www.jetbrains.com/idea/]((http://www.jetbrains.com/idea/)

2. 注册intelli idea
打开已经安装好的intelli idea，通过它提供的License server进行注册，选择License server方式后，在文本框内输入“http://www.iteblog.com/idea/key.php”点击“OK”即可。


## hello world

1. 新建工程文件，打开intelli idea，选择菜单栏中“file”→“new”→“project”命令，新建一个project,在project SDK中导入前面安装的JDK，然后一直next，最后命名完成即可。

2. 新建类，在工程文件下的src上右击，选择“new”→“javaclass”,输入类名，在类文件中输入：
```java
public static void main(String[ ] args) {
    System.out.println("Hello World");
}
```
快捷键`ctrl`+`shift`+`F10`，或者点击工具栏的run和debug按钮，即可编译运行代码。


如果运行结果为“hello world”，则说明运行成功。









我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！