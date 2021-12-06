title: 关于viewport
date: 2015-12-03 11:25:31
tags: [web,H5]
description: 响应式布局,viewport.html5

---

本文是我学习了无双的【[移动前端开发之viewport的深入理解](http://www.cnblogs.com/2050/p/3877280.html)】后，所做的学习笔记。

## 1.什么是viewport？
移动设备上的viewport就是设备的屏幕上能用来显示我们的网页的那一块区域，在具体一点，就是浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。

<!-- more -->

## 2.css中的px与设备物理px的关系
css的px并不等于物理的px。css中的像素只是一个抽象的单位，在不同的设备或不同的环境中，css中的1px所代表的设备物理像素是不同的。这个因素在为桌面浏览器设计的网页中影响不大，但在移动设备中，就要考虑这个因素了。

## 3.三个viewport概念
+ layout viewport：浏览器默认的viewport。这个layout viewport的宽度可以通过 document.documentElement.clientWidth 来获取。

+ visual viewport：浏览器可视区域的大小

+ ideal viewport：能完美适配移动设备的viewport。
	+ 所谓的完美适配指的是，首先不需要用户缩放和横向滚动条就能正常的查看网站的所有内容；第二，显示的文字的大小是合适，比如一段14px大小的文字，不会因为在一个高密度像素的屏幕里显示得太小而无法看清，理想的情况是这段14px的文字无论是在何种密度屏幕，何种分辨率下，显示出来的大小都是差不多的。当然，不只是文字，其他元素像图片什么的也是这个道理。
	+ 每个移动设备浏览器中都有一个理想的宽度，这个理想的宽度是指css中的宽度，跟设备的物理宽度没有关系，在css中，这个宽度就相当于100%的所代表的那个宽度

![](/images/201511/3.jpg)
## 4.viewport控制
下面这段代码可以实现对浏览器viewport的控制

	<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">

属性参数：

+ width:设置layout viewport  的宽度，为一个正整数，或字符串"width-device"
+ initial-scale:设置页面的初始缩放值，为一个数字，可以带小数。
+ minimum-scale:允许用户的最小缩放值，为一个数字，可以带小数
+ maximum-scale:允许用户的最大缩放值，为一个数字，可以带小数
+ height	设置layout viewport  的高度，这个属性对我们并不重要，很少使用
+ user-scalable:是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许

<b style="color:red">initial-scale缩放是相对于 ideal viewport来进行缩放，即width值。</b>

把当前的viewport宽度设置为 ideal viewport 的宽度并兼容所有设备的写法：

	<meta name="viewport" content="width=device-width, initial-scale=1">

 因为（initial-scale=1 ）（width=device-width）虽然都能把当前的viewport宽度变成 ideal viewport 的宽度，但（width=device-width）在iphone和ipad上，无论是竖屏还是横屏，宽度都是竖屏时ideal viewport的宽度，（initial-scale=1 ）在windows phone 上的IE 无论是竖屏还是横屏都把宽度设为竖屏时ideal viewport的宽度。<b style="color:red">但当俩者同时出现时，浏览器会取它们两个中较大的那个值。</b>


