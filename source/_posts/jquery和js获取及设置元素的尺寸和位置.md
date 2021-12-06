---
title: jquery和js控制元素的尺寸和位置
date: 2015-04-28 22:38:14
description: jquery和js控制样式，位置，尺寸
tags: [web,javascript]
categories: [js]
---

### 1.偏移量
_javascript_：

* offsetLeft：元素的左外边框至offsetParent的左内边框之间的像素距离。
* offsetTop：元素的上外边框至offsetParent的上内边框之间的像素距离。
* 这里的offsetParent为距离当前元素最近的拥有定位的父元素：
　　1.拥有定位指的是position为absolute或relative
　　2.如果当前元素的父级元素并不拥有定位，offsetParent为body。

<!-- more -->

---

_jquery_：

* position()：
　　*返回的对象包含两个整型属性：top 和 left；

　　*可通过ele.position().top和ele.position().left单独获取和设置
* offset():

　　*获取匹配元素在当前视口的相对偏移。
![](/images/201511/8.png)

<i> 上图盗自《javascript高级程序设计》</i>

### 2.尺寸值
_javascript_：

* offsetHeight：元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的）水平滚动条的高度、上边框高度和下边框高度。<b style="color:red">只能获取，不能设置</b>。
* offsetWidth：元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂直滚动条的宽度、左边框宽度和右边框宽度。<b style="color:red">只能获取，不能设</b>。
* ele.style.width  获取元素content的宽度。不包括补白和边距、边框
* ele.style.height  获取元素content的高度。不包括补白和边距、边框
* clientWidth  元素内容区宽度加上左右内边距宽度
* clientHeight  元素内容区高度加上上下内边距高

---
_jquery_：

* outerHeight()      
　　与javascript的offsetHeight功能一致
* outerWidth()       
　　与javascript的offsetWidth功能一致
* width()
　　与javascript的ele.style.width功能一致
* height()           
　　与javascript的ele.style.height功能一致
* innerWidth()       
　　与javascript的clientWidth功能一致
* innerHeight()      
　　与javascript的clientHeight功能一致
![](/images/201511/9.png)

<i>上图盗自《javascript高级程序设计》</i>

### 2.滚动大小
_javascript_

* scrollHeight：在没有滚动条的情况下，元素内容的总高度。=scrollTop+可视区域高度
* scrollWidth：在没有滚动条的情况下，元素内容的总宽度。=scrollLeft+可视区域宽度
* scrollLeft：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。
* scrollTop：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。

_jquery_：

* scrollTop()  获取匹配元素相对滚动条顶部的偏移，同js
* scrollLeft()  获取匹配元素相对滚动条左侧的偏移，同js
![](/images/201511/10.png)
<i>上图，你懂的</i>

---
<p style="text-align:right">整理于2015-11-26 16:13:14</p>