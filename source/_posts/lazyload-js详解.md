title: lazyload.js详解
date: 2015-12-16 14:23:21
tags: [插件]
description:

---

## 简介

lazyload.js用于长页面图片的延迟加载，视口外的图片会在窗口滚动到它的位置时再进行加载，这是与预加载相反的。

## 优点：

+ 它可以提高页面加载速度；
+ 在某些情况清晰它也可以帮助减少服务器负载。

<!-- more -->
## 安装

bower安装：
``` bash
$ bower install jquery.lazyload
```

npm安装:
``` bash
$ npm install jquery-lazyload  
```



## 使用
lazyload依赖与jquery。所以先引入jquery和lazyload

	<script src="jquery.js"></script>
	<script src="jquery.lazyload.js"></script>

1.将图片路径写入data-original属性
2.给lazyload的图片增加一个名为lazy的class
3.选择所有要lazyload的图片（img.lazy），执行lazyload();

	<img class="lazy" data-original="img/example.jpg" style="margin-top:1000px" height="200">
	<script>
		$(function(){
			$("img.lazy").lazyload();
		})
	</script>
<p style="color:red">注意：必须设置图片的高度或者宽度，否则插件可能无法正常工作</p>

## 提前加载——Threshold
lazyload默认是当滚动到该图片位置时，加载该图片。但是可以通过设置Threshold参数来实现滚到距离其xx px时就加载。

	
		$(function(){
			$("img.lazy").lazyload({
				threshold :20
			});
		})
	

上面的例子设置了滚动到距离图片20px时，图片就开始再开始加载

## 事件触发(可以是jquery事件，也可以是自定义事件)——Event

当触发定义的事件时，图片才开始加载

	
		$(function(){
			$("img.lazy").lazyload({
				event : "click"
			});
		})
	
上面的例子使图片点击后，才开始加载


<p style="color:rgb(27, 128, 158)">Tip:你可以使用这个来实现图片的延迟加载</p>

	$(function() {
	    $("img.lazy").lazyload({
	        event : "sporty"
	    });
	});

	$(window).bind("load", function() {
	    var timeout = setTimeout(function() {
	        $("img.lazy").trigger("sporty")
	    }, 5000);
	});

上面的代码在页面加载完毕后五秒才开始加载图片


## 设定效果——Effects

插件默认的加载效果是 `show()` ,你可以使用任何你想要的效果。下面的代码使用了 `fadeIn()`

	$("img.lazy").lazyload({
	    effect : "fadeIn"
	});

## 滚动容器内的图片——container

插件也可以使用在滚动容器内的图片上。下面的div拥有scrollerbar，内容的内容进行滚动，滚到图片位置时，图片开始加载

	<div style="height:600px;overflow:scroll" id="container">
		<img class="lazy" data-original="img/example.jpg"  alt="" style="margin-top:1000px" height="200">
	</div>
	<script>
		$(function(){
			$("img.lazy").lazyload({
				container: $("#container")
			});
		})
	</script>

## 不顺序排列的图片——failurelimit
+ 插件会执行一个寻找未加载图片的循坏，该循环会检查图片是否可见，默认情况下，当第一个视图外的图片被找到，循环就会停止 。
+ 但是存在一种情况：页面布局图片的顺序和html图片代码的顺序不一致;它会导致本该加载的没有加载。这种情况下就可以将 failurelimit 设为 10 ，它令插件找到 10 个不在可见区域的图片是才停止搜索. 如果你有一个恶心的布局, 请把这个参数设高一点。

代码：

	$("img.lazy").lazyload({
	    failure_limit : 10
	});

## 处理隐藏图片——skip_invisible 

为了提升性能，插件默认忽略隐藏的图片；如果想要加载隐藏图片.设置skip_invisible为false;
<p style="color:red">注意：Webkit浏览器将自动把没有宽度和高度的图像视为不可见</p> 

	$("img.lazy").lazyload({
	    skip_invisible : true
	});


详情请查看[文档](http://www.appelsiini.net/projects/lazyload/)