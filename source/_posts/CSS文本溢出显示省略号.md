---
title: CSS文本溢出显示省略号
date: 2015-12-30 15:30:50
tags: [css]
description: CSS文本溢出显示省略号
categories: css
---

项目中常常有这种需要我们对溢出文本进行"..."显示的操作，单行多行的情况都有（具体几行得看设计师心情了），这篇随笔是我个人对这种情况解决办法的归纳，欢迎各路英雄指教。
<!-- more -->

## 单行

<span style="color:#C0F">语法</span>

```CSS
	overflow:hidden;
	text-overflow:ellipsis;
	white-space:nowrap
```

<span style="color:#0A8">示例</span>

<p style="width:250px;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;background-color: #f0ad4e;">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号
</p>

## 多行

**1.直接用css属性设置(只有-webkit内核才有作用)**

<span style="color:#C0F">语法</span>

```CSS
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
```

<span style="color:#d44950">移动端浏览器绝大部分是WebKit内核的，所以该方法适用于移动端；</span>

+ -webkit-line-clamp  用来限制在一个块元素显示的文本的行数,这是一个不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。

+ display: -webkit-box 将对象作为弹性伸缩盒子模型显示 。

+ -webkit-box-orient 设置或检索伸缩盒对象的子元素的排列方式 。

+ text-overflow: ellipsis  以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本。

<span style="color:#0A8">示例</span>
<p style="
  width:250px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
	background-color: #f0ad4e;
  ">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略</p>

**2.利用伪类**

<span style="color:#C0F">语法</span>
```HTML
<div id="con">
  <span id="txt">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略</span>
  <span class="t"></span>
</div>
<style>
#txt{
  display: inline-block;
  height: 40px;
  width: 250px;
  line-height: 20px;
  overflow: hidden;
  font-size: 16px;
}
.t:after{
  display: inline;
  content: "...";
  font-size: 16px;
	
}
</style>
```

<span style="color:#0A8">示例</span>

<style>
	#con{
	background-color: #f0ad4e;
    }
	#txt{
		display: inline-block;
		height: 40px;
		width: 250px;
		line-height: 20px;
		overflow: hidden;
	}
	.t:after{
		display: inline;
		content: "...";
	}
</style>
<p id="con"><span id="txt">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略</span><span class="t"></span>
</p>



**3.利用绝对定位和padding;(跨浏览器解决方案)**
看到上例是不是觉得“哇，终于可以跨浏览器使用了”，但你这样想的时候有没有考虑过IE的感受？IE6/7是没有伪类的，还不赶快跪下对IE叫声“大哥”，虽然IE6已经退出市场，但是IE7还是需要兼容的，所以呢，我自己又想到了以下的办法，我这边测试了下感觉还行。

<span style="color:#C0F">上代码</span>

```
<p id="con2">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略<span class="t2">...</span>
</p>
<style>
#con2{
  position: relative;
  height: 40px;
  width: 250px;
  line-height: 20px;
  overflow: hidden;
  padding-right: 12px;
}  
.t2{
  position: absolute;
  right: 0;
  bottom: 0;
}
</style>
```
这个方法的原理是：首先在包含文字的元素里，嵌入一个`<span>...</span>`，然后包含文字的元素右侧留出`...`的位置(`padding-right`),最后利用绝对定位将`...`定位至右侧的`padding-right`区域
<span style="color:#0A8">示例</span>


<p id="con2">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略<span class="t2">...</span>
</p>
<style>
  #con2{
  	position: relative;
  	height: 40px;
  	width: 250px;
  	line-height: 20px;
  	overflow: hidden;
  	padding-right: 12px;
  	background-color: #f0ad4e;
  }  
  .t2{
  	position: absolute;
  	right: 0;
  	bottom: 0;
  }
</style>


**4.其他**
利用js插件来实现该功能，这里有俩款插件推荐，这篇主要介绍的是css方法，所以它们使用方法就不废话了。

+ [Clamp.js](https://github.com/bigdots/Clamp.js)
+ [jQuery.dotdotdot](https://github.com/bigdots/jQuery.dotdotdot)