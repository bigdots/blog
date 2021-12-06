---
title: iScroll-js—"smooth scrolling for the web"
date: 2015-12-15 15:09:11
tags: [插件,移动端开发]
description: iScroll
categories: [js]
---

## 一、什么是iScroll

[官方文档](http://iscrolljs.com/)的解释：
`iScroll`是一个高性能、占用空间小、依赖免费的多平台javascrip脚本库。它应用于桌面设备、移动设备和智能电视。它一直在大力优化性能和大小以便在现代设备和旧设备上提供流畅的运行。`iScroll`不仅仅只是用来滚动。它还可以处理任何用户交互需要的元素移动。它增加了滚动,缩放,平移,无限滚动、视差滚动、旋转木马,并且它仅仅只有4 kb。

<!-- more -->
## 二、为什么要用iScroll
由于不同的手机版本造成页面的弹性滚动条兼容性不好，iscroll封装了弹性滚动条插件可以跨手机版本实现弹性滚动UI设计

## 三、Getting started

注意事项：

+ `IScroll`需要对所要进行滚动的元素进行初始化。
+ `iScroll`的数量没有限制，你可以在每个页面使用任意数量的`iScroll`(如果硬件允许的话)　　
+ 保持DOM尽可能简单。最优的HTML结构是:

滚动区域必须包裹在Iscroll中。在上面的例子中,UL元素将滚动。只有第一个子元素是滚动的,额外的子元素则会被忽略掉。

	<div id="wrapper">
	    <ul>
	        <li>...</li>
	        <li>...</li>
	        ...
	    </ul>
	</div>

---

实例化：

	<script type="text/javascript">
		var wrapper = document.getElementById('wrapper');
		var myScroll = new IScroll(wrapper);
	</script>

<p style="color:red">注意：
1.iScroll使用querySelector而不是querySelectorAll,所以选中的是第一个选择器元素。如果你需要iScroll适用于多个对象你要构建循环。
2.iScroll需要在DOM加载完毕时启动,最安全的办法是绑定在窗口onload事件。
3.iScroll需要知道滚动区域的高度/宽度
</p>


## 四、配置iScroll
在初始化阶段可以通过第二个参数来配置iScroll

	var myScroll = new IScroll('#wrapper', {
	    mouseWheel: true,
	    scrollbars: true
	});

上面的示例打开了鼠标滚轮和滚动条支持。

常用参数：
`hScroll` :                false 禁止横向滚动 true横向滚动 默认为true
`vScroll`  :               false 精致垂直滚动 true垂直滚动 默认为true
`hScrollbar`:            false隐藏水平方向上的滚动条
`vScrollbar` :           false 隐藏垂直方向上的滚动条
`fixedScrollbar`:      在iOS系统上，当元素拖动超出了scroller的边界时，滚动条会收缩，设置为true可以禁止滚动条超出scroller的可见区域。默认在Android上为true， iOS上为false
`fadeScrollbar`  :　  false 指定在无渐隐效果时隐藏滚动条
`hideScrollbar`  :　  在没有用户交互时隐藏滚动条 默认为true
`bounce`         :  　启用或禁用边界的反弹，默认为true
`momentum`   　 :启用或禁用惯性，默认为true，此参数在你想要保存资源的时候非常有用
`lockDirection`    :   false取消拖动方向的锁定， true拖动只能在一个方向上（up/down 或者left/right）