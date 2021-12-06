# CSS布局基础——BFC #

## what's BFC? ##
第一次看到这个名词，我是拒绝的，css什么时候还有这个东西？于是迫不及待的google了一下，才发现原来它无时无刻不在我们的css当中，只不过它并不是一个属性，不需要我们平常使用手写罢了。但是它的重要性确是杠杠的，可以这么说，没有它就就没有什么css布局。
BFC,全称 Block Formatting Context，翻译成块级格式化上下文，它就是一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。


看一大堆文字可能有点抽象，现在拿个js函数来比喻说明一下吧，我们现在有一个叫做bfc的函数，而一个函数就是一个块级作用域，这里面所有的变量申明、运行都在这个块级作用域内进行。理所当然，一个环境中的变量不会影响到其它环境变量。

```js
	var box =1;
	function bfc(){
		var box = "2";
		console.log(box);
	}
	bfc();//2
	console.log(box)//1
```

所以，我们是不是可以这样理解：所谓的BFC就是css属性的执行域？


## BFC的生成 ##

既然js可以通过函数等方法来实现块级作用域，我想那css肯定也是可以通过一些手段来实现BFC的。
这里BFC的官方文档写到：
> Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.

从这段描述可以清楚知道，以下方法可以创建一个新的块级执行上下文（BFC）：

1. 浮动元素、
2. 绝对定位元素，
3. 块级元素以及块级容器(比如inline-block、table-cell、table-capation)
4. overflow值不为visible的块级盒子

*当然，root元素会自动生成一个BFC，这个应该很好理解,毕竟需要一个根BFC来布局*


## 执行规则 ##
既然存在了执行环境，那肯定会存在执行规则。BFC的

**1.在一个块级排版上下文中，盒子是从包含块顶部开始，垂直的一个接一个的排列的。每个盒子的左外边是触碰到包含块的左边的（对于从右向左的排版，则相反）**

这个应该不难理解。就是我们如果在<body></body>里写几个`<div>`，它会依次垂直排列，并且都是在页面的最左边（对于从右向左的排版，则相反）。

**2.相邻两个盒子之间的垂直的间距是被margin属性所决定的，在一个块级排版上下文中相邻的两个块级盒之间的垂直margin是折叠的。**

这句描述是不是超级熟悉，这不是我css常见的边距折叠问题吗？现在知道它出自哪里了吧，就是这里。下面的俩个盒子各有上下20px的间距，加起来应该有40px,但显然，现在只有20px;

```css
    <style>
	.top{
    	width:100px;
		height:100px;
		background:#000;
		margin:20px 0;
	}
	.bottom{
		width:100px;
		height:100px;
		background:#000;
		margin:20px 0;
	}
    </style>
    <div  class="top"></div>
    <div  class="bottom"></div>

<style>
.top{
	width:100px;
	height:100px;
	background:#ADD9E6;
	margin:20px 0;
}
.bottom{
	width:100px;
	height:100px;
	background:#FFCCCC;
	margin:20px 0;
}
</style>
<div  class="top"></div>
<div  class="bottom"></div>
```
	发生边距折叠是因为同一个BFC的关系(根BFC)。既然知道原因，解决就好办了，让他们俩个不在同一个BFC就ok啦。
**3. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。**

通过这条属性，我们又可以想到哪些呢。对，浮动元素的塌陷问题。我们知道，一个元素中的子元素浮动了，这个父元素就会发生高度塌陷问题。下例中一旦内部的红色元素浮动，蓝色的盒子就无法被撑起，高度会变成0。
```css
	<style>
	.wrap{
		width:150px;
		background:#ADD9E6;
		margin:20px 0;
	}
	.in{
		width:100px;
		height:100px;
		background:#FFCCCC;
		margin:20px 0;
		//float:left;
	}
	</style>
	<div class="wrap"><div class="in"></div></div>

<style>
	.wrap{
		width:150px;
		background:#ADD9E6;
		margin:20px 0;
	}
	.in{
		width:100px;
		height:100px;
		background:#FFCCCC;
		margin:20px 0;
		//float:left;
	}
</style>
<div class="wrap"><div class="in"></div></div>
```
现在我们知道了，这是因为浮动元素创建了一个新的BFC，成为了一个独立的容器，不会影响到外面的父元素了。它的定位规则不再受制于这个父元素了。如何解决这一问题？我们知道只要在在父元素加上`overflow:hidden`可以清除浮动。但是这又是为什么？

其实，这就是前面提到的`overflow:hidden`可以生成一个新的BFC，而这个浮动的子元素，被它所包含了，从而成为一个独立容器，它的float外溢不了了，外面的元素不再受其浮动的影响，从而达到了清除浮动的作用。



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！