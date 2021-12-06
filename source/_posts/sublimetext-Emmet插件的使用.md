---
title: sublimetext Emmet插件的使用
date: 2015-05-06 22:53:0
tags: [web,工具]
description:

---categories: [js]

### 1. 省略div，插件会默认元素为div

.container

	<div class="container"></div>

### 2. 含糊标签名称,不需要指定。比如li，Emmet会自动帮助你生成 
 ul>.lis

	<ul>
	<li class="lis"></li>
	</ul>

<!-- more -->

### 3.链式缩写 

① > :添加创建子元素
div>span 

	<div><span></span></div>

② + :添加创建同层级元素

div+span

	<div></div>
	<span></span>
③ ^ :添加一个父层级元素（需要的话你可以向上多层）

 p>a^p 

	<p><a href=""></a></p>
	<p></p>
 


### 4.分组功能；
 (.one>h1)+(.two>h1) 


	<div class="one">
	<h1></h1>
	</div>
	<div class="two">
	<h1></h1>
	</div>

### 5.插入文本{}和属性[]；

h1{heading}

	<h1>heading</h1>
	 a[href='#'] 

	<a href="#"></a>
### 6.添加多个class；

 .one.two.three

	<div class="one two three"></div>
### 7.添加多个元素
 ul>li*5 


	<ul>
	<li></li>
	<li></li>
	<li></li>
	<li></li>
	<li></li>
	</ul>

### 8.自动列表计数

 按顺序生成HTML元素，使用 ``$``符号可以帮助生成一系列数字，支持class，id，属性，内容等等。
(生成n位的数字，使用n个``$``符号)
ul>li.list``$``{number``$``}*5


	<ul>
	<li class="list1">number 1</li>
	<li class="list2">number 2</li>
	<li class="list3">number 3</li>
	<li class="list4">number 4</li>
	<li class="list5">number 5</li>
	</ul>

ul>li$.number``$$``*5 


	<ul>
	<li1 class="number01"></li1>
	<li2 class="number02"></li2>
	<li3 class="number03"></li3>
	<li4 class="number04"></li4>
	<li5 class="number05"></li5>
	</ul>

---
<p style="text-align:right">整理于2015-11-30 16:31:18</p>