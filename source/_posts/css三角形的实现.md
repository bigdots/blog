---
title: css三角形的实现
date: 2015-07-07 18:22:32
tags: [css]
description: css三角形
categories: [css]
---

### 实心三角形
原理：

![](/images/201511/1.jpg)
<!-- more -->

上图为盒子的边框模型。当中间的conten部分的宽和高为0时，就变成了下图这样。

![](/images/201511/2.jpg)
接着，我们让左右边框为透明，上边框为0；即为如下三角形
<div style="width:0;height:0;border-left: 160px solid 
            transparent;border-right: 160px solid transparent;
			border-bottom: 130px solid #eeeeee;">
			</div>

_代码实现_

		div{
			width:0;height:0;
			border-left: 160px solid transparent;
            border-right: 160px solid transparent;
			border-bottom: 130px solid #eeeeee;
		}
	



网上有人做了个动图，解释了全过程
![](/images/201511/2.gif)


### 空心三角形

原理：用俩个三角形进行叠加
<style>
	.triangle:before{
	        top:0;
	        left:0;
	        content: '';
	        position: absolute;
	        display: block;
	        width: 0;
	        height: 0;
	        border-left: 50px solid transparent;
	        border-right: 50px solid transparent;
	        border-bottom: 50px solid red;
	    }
	    .triangle:after{
	        top:1px;
	        left:2px;
	        content: '';
	        position: absolute;
	        display: block;
	        width: 0;
	        height: 0;
	        border-left: 48px solid transparent;
	        border-right: 48px solid transparent;
	        border-bottom: 48px solid #fff;
	    }
	    .triangle{
	        position: relative;
	        height: 60px;
	    }
</style>

<div class="triangle"></div>

	
代码实现

	<div class="triangle"></div>

	.triangle:before{
        top:0;
        left:0;
        content: '';
        position: absolute;
        display: block;
        width: 0;
        height: 0;
        border-left: 50px solid transparent;
        border-right: 50px solid transparent;
        border-bottom: 50px solid red;
    }

    .triangle:after{
        top:1px;
        left:2px;
        content: '';
        position: absolute;
        display: block;
        width: 0;
        height: 0;
        border-left: 48px solid transparent;
        border-right: 48px solid transparent;
        border-bottom: 48px solid #fff;
    }

    .triangle{
        position: relative;
    }
	

---
<p style="text-align:right">整理于2015-11-30 14:08:14</p>