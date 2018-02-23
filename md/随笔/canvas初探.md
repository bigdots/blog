title: canvas初探(内含时钟)
date: 2015-12-04 13:06:50
tags: [web,H5]
description: html5,Canvas,时钟范例

---

## 一、了解Canvas

`<canvas> `是为了客户端矢量图形而设计的。它自己没有行为，但却把一个绘图 API 展现给客户端 JavaScript 以使脚本能够把想绘制的东西都绘制到一块画布上。

大多数 Canvas 绘图 API 都没有定义在	`<canvas> `元素本身上，而是定义在通过画布的 getContext() 方法获得的一个“绘图环境”对象上。

Canvas API 也使用了路径的表示法。但是，路径由一系列的方法调用来定义，而不是描述为字母和数字的字符串，比如调用 beginPath() 和 arc() 方法。
一旦定义了路径，其他的方法，如 fill()，都是对此路径操作。绘图环境的各种属性，比如 fillStyle，说明了这些操作如何使用。

<!-- more -->

### 属性和方法详情可以翻阅[参考手册](http://www.w3school.com.cn/tags/html_ref_canvas.asp)

## 二、基本图形的绘制

### 1. 矩形
<canvas id="my_canvas1" height="80"></canvas>
<script>
	var a = document.getElementById('my_canvas1');
	var txt = a.getContext('2d');
	txt.fillStyle="red"; 
	txt.fillRect(0,0,150,75) 
</script>


代码解析

	<canvas id="my_canvas1" height="80"></canvas>
	<script>
		var a = document.getElementById('my_canvas1');
		//创建getContext对象,它是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法
		var axt = a.getContext('2d');
		//fillStyle属性设置或返回用于填充绘画的颜色、渐变或模式
		txt.fillStyle="red"; 
		//fillRect(x,y,w,h)绘制“被填充”的矩形。参数分别为(起始位置横坐标，纵坐标,宽度,高度)
		txt.fillRect(0,0,150,75) 
	</script>


### 2.路径

<canvas id="my_canvas2" height="60"></canvas>
<script>
	var b = document.getElementById('my_canvas2');
	var bxt = b.getContext('2d');
	bxt.beginPath()
	bxt.moveTo(10,10); 
	bxt.lineTo(150,50); 
	bxt.lineTo(10,50);
	bxt.closePath();
	bxt.stroke();
</script>

代码解析

	<canvas id="my_canvas2" height="60"></canvas>
	<script>
	var b = document.getElementById('my_canvas2');
	var bxt = b.getContext('2d');
	//起始一条路径，或重置当前路径
	bxt.beginPath()
	//把路径移动到画布中的指定点，不创建线条
	bxt.moveTo(10,10);
	//添加一个新点，然后在画布中创建从该点到最后指定点的线条 
	bxt.lineTo(150,50); 
	bxt.lineTo(10,50);
	//创建从当前点回到起始点的路径
	bxt.closePath();
	//绘制已定义的路径
	bxt.stroke();
</script>

### 3.圆

<canvas id="my_canvas3" height="40"></canvas>
<script>
	var e = document.getElementById('my_canvas3');
	var cxt = e.getContext('2d');
	cxt.beginPath();
	cxt.arc(70,18,15,0,Math.PI*2,true);
	cxt.closePath();
	cxt.stroke();
</script>

代码解析

	<canvas id="my_canvas3" height="40"></canvas>
	<script>
		var e = document.getElementById('my_canvas3');
		var cxt = e.getContext('2d');
		cxt.beginPath();
		<!--
		arc(x,y,r,sAngle,eAngle,counterclockwise)	创建弧/曲线（用于创建圆形或部分圆）
		x	圆心的 x 坐标。
		y	圆心的 y 坐标。
		r	圆半径。
		sAngle	起始角，以弧度计。（弧的圆形的三点钟位置是 0 度）。
		eAngle	结束角，以弧度计。
		counterclockwise	可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针。
		Math.PI*2:圆周率
		 -->
		cxt.arc(70,18,15,0,Math.PI*2,true);
		cxt.closePath();
		cxt.stroke();
	</script>

### 4.文本

<canvas id="my_canvas4" height="20"></canvas>
<script>
	var f = document.getElementById('my_canvas4');
	var axt = f.getContext('2d');
	axt.font="30px Arial";
	axt.strokeText("hello world",150,20)
</script>

代码解析

	<canvas id="my_canvas4" height="20"></canvas>
	<script>
		var f = document.getElementById('my_canvas4');
		var axt = f.getContext('2d');
		//设置或返回文本内容的当前字体属性
		axt.font="30px Arial";
		//在画布上绘制文本（无填充）
		axt.strokeText("hello world",150,20)
	</script>
<b style="color:red">注意：font属性设置语句必须置于绘制文本方法(strokeText)之前</b>

### 5.渐变

+ 线性渐变

<canvas id="my_canvas5" height="20"></canvas>
<script>
	var g = document.getElementById('my_canvas5');
	var gxt = g.getContext('2d');
	var grd=gxt.createLinearGradient(0,0,200,0);
	grd.addColorStop(0,"red");
	grd.addColorStop(1,"white");
	gxt.fillStyle=grd;
	gxt.fillRect(10,10,150,20);
</script>

代码解析

	<canvas id="my_canvas5" height="20"></canvas>
	<script>
		var g = document.getElementById('my_canvas5');
		var gxt = g.getContext('2d');
		//createLinearGradient(x,y,x1,y1) - 创建线性渐变,坐标是起点和终点
		var grd=gxt.createLinearGradient(0,0,200,0);
		//addColorStop()方法规定渐变对象中的颜色和停止位置，可以是0至1.
		grd.addColorStop(0,"red");
		grd.addColorStop(1,"white");
		gxt.fillStyle=grd;
		gxt.fillRect(10,10,150,20);
	</script>

---

+ 径向渐变

<canvas id="my_canvas6" height="50"></canvas>
<script>
	var h = document.getElementById('my_canvas6');
	var hxt = h.getContext('2d');
	var hrd=gxt.createRadialGradient(25,25,5,25,25,25);
	hrd.addColorStop(0,"red")
	hrd.addColorStop(1,"white");
	hxt.fillStyle=hrd;
	hxt.arc(25,25,25,0,Math.PI*2,true);
	hxt.fill()
</script>


代码解析

	<canvas id="my_canvas6" height="50"></canvas>
	<script>
		var h = document.getElementById('my_canvas6');
		var hxt = h.getContext('2d');
		<!-- 
		createRadialGradient(x,y,r,x1,y1,r1) - 创建一个径向/圆渐变- 
		x	渐变开始点的 x 坐标
		y	渐变开始点的 y 坐标
		x1	渐变结束点的 x 坐标
		y1	渐变结束点的 y 坐标 
		r   渐变开始点的渐变半径
		r1  渐变结束点的渐变半径
		-->
		var hrd=gxt.createRadialGradient(25,25,5,25,25,25);
		hrd.addColorStop(0,"red")
		hrd.addColorStop(1,"white");
		hxt.fillStyle=hrd;
		hxt.arc(25,25,25,0,Math.PI*2,true);
		hxt.fill()
	</script>


### 6.图像
<canvas id="my_canvas7" ></canvas>
<script>
	var j=document.getElementById("my_canvas7");
	var jxt=j.getContext("2d");
	var img1=new Image()
	img1.src="http://bigdots.github.io/img/thumb.jpg";
	img1.onload = function(){
		/*
		drawImage(img,sx,sy,swidth,sheight,x,y,width,height)向画布上绘制图像、画布或视频
		img	规定要使用的图像、画布或视频。
		sx	可选。开始剪切的 x 坐标位置。
		sy	可选。开始剪切的 y 坐标位置。
		swidth	可选。被剪切图像的宽度。
		sheight	可选。被剪切图像的高度。
		x	在画布上放置图像的 x 坐标位置。
		y	在画布上放置图像的 y 坐标位置。
		width	可选。要使用的图像的宽度。（伸展或缩小图像）
		height	可选。要使用的图像的高度。（伸展或缩小图像）
		*/
		jxt.drawImage(img1,0,0);
	}
</script>



代码解析

	<canvas id="my_canvas7" ></canvas>
	<script>
		var j=document.getElementById("my_canvas7");
		var jxt=j.getContext("2d");
		var img1=new Image()
		img1.src="http://bigdots.github.io/img/thumb.jpg";
		img1.onload = function(){
			/*
			drawImage(img,sx,sy,swidth,sheight,x,y,width,height)向画布上绘制图像、画布或视频
			img	规定要使用的图像、画布或视频。
			sx	可选。开始剪切的 x 坐标位置。
			sy	可选。开始剪切的 y 坐标位置。
			swidth	可选。被剪切图像的宽度。
			sheight	可选。被剪切图像的高度。
			x	在画布上放置图像的 x 坐标位置。
			y	在画布上放置图像的 y 坐标位置。
			width	可选。要使用的图像的宽度。（伸展或缩小图像）
			height	可选。要使用的图像的高度。（伸展或缩小图像）
			*/
			jxt.drawImage(img1,0,0);
		}
	</script>

<b style="color:red">因为我这里是通过js设置src加载图片，可能会存在图片没有加载成功，就执行了drawImage，导致画布上没有图片，所以，我用了onload</b>


## 三、时钟范例

<canvas id="time" width="520" height="520"></canvas>
<script>
	var mycanvas = document.getElementById('time');
	var time = mycanvas.getContext('2d');

	function clock(){
		time.clearRect(0, 0, 800, 800);

		//获取时间
		var now = new Date();
		var secd = now.getSeconds();
		var min = now.getMinutes();
		var hour = now.getHours();

		//转为12时制
		hour = hour > 12 ? hour-12 : hour;

		//先画个大圆
		time.beginPath();
		time.lineWidth = 10;
		time.strokeStyle = "#000";
		time.arc(250, 250, 200, 0, 360, false);
		time.closePath();
		time.stroke();

		//画出刻度
		for(var i=0;i<12;i++){
			//保存当前环境的状态
			time.save();
			//设置粗细和颜色
			time.lineWidth = 6;
			time.strokeStyle = '#000';

			//translate(x,y) 方法重新映射画布上的 (0,0) 位置。
			//x	添加到水平坐标（x）上的值
			// y	添加到垂直坐标（y）上的值
			time.translate(250, 250);
			//rotate() 方法旋转当前的绘图，以弧度计。
			
			 time.font = "20px Verdana";//必须前置
			 time.fillText('12',-12,-150);
			 time.fillText('3',150,6);
			 time.fillText('6',-6,160);
			 time.fillText('9',-150,6);

			time.rotate((i * 30) * Math.PI / 180);//角度*Math.PI/180=弧度
			time.beginPath();
			time.moveTo(0, -170);
			time.lineTo(0, -190);
			time.closePath();

			time.stroke();
			
			time.restore();
		}


		/*
		1.save() 方法保存当前图像状态的一份拷贝。
		2.设置刻度的粗细和颜色
		3.利用translate把新的参照坐标设为(250,250)，即下面的坐标就是以(250,250)【圆心】为参照了；所以下面的(0,-170)其实就是(250,70)
		4.旋转30度【360/12=每个小时的度数】
		5.画线，(0, -170)至(0, -190)的线条，其实就是12点的那个刻度，然后通过旋转和循环来画出所有刻度
		6.restore() 方法将绘图状态置为保存值。
		 */

		//时针
			time.save();
			time.lineWidth = 7;
			time.strokeStyle = "black";
			time.translate(250, 250);
			//【360/12】设定指针指向位置
			time.rotate(hour * 30 * Math.PI / 180);
			time.beginPath();
			time.moveTo(0, -140);
			time.lineTo(0, 10);
			time.stroke();
			time.closePath();
			time.restore();
		//分针
			time.save();
			time.lineWidth = 5;
			time.strokeStyle = "black";
			time.translate(250, 250);
			//【360/60】
			time.rotate(min * 6 * Math.PI / 180);
			time.beginPath();
			time.moveTo(0, -160);
			time.lineTo(0, 10);
			time.stroke();
			time.closePath();
			time.restore();
		//秒针
			time.save();
			time.lineWidth = 3;
			time.strokeStyle = "red";
			time.translate(250, 250);
			//【360/60】
			time.rotate(secd * 6 * Math.PI / 180);
			time.beginPath();
			time.moveTo(0, -170);
			time.lineTo(0, 10);
			time.closePath();
			time.stroke();
		//画交叉点
			time.beginPath();
			time.arc(0, 0, 5, 0, 360, false);
			time.closePath();
			time.fillStyle = "gray";
			time.fill();
			time.stroke();
			time.beginPath();
			time.arc(0, -150, 5, 0, 360, false);
			time.closePath();
			time.fillStyle = "gray";
			time.fill();
			time.stroke();
			time.restore();
		

	}

	clock();
	setInterval(clock,1000)
</script>

基础也看完了，是时候动手了。上面是一个canvas写的时钟。just do it!


	<canvas id="time" width="520" height="520"></canvas>
	<script>
		var mycanvas = document.getElementById('time');
		var time = mycanvas.getContext('2d');

		function clock(){
			time.clearRect(0, 0, 800, 800);
			//获取时间
			var now = new Date();
			var secd = now.getSeconds();
			var min = now.getMinutes();
			var hour = now.getHours();
			//转为12时制
			hour = hour > 12 ? hour-12 : hour;
			//先画个大圆
			time.beginPath();
			time.lineWidth = 10;
			time.strokeStyle = "#000";
			time.arc(250, 250, 200, 0, 360, false);
			time.closePath();
			time.stroke();
			//画出刻度
			for(var i=0;i<12;i++){
				//保存当前环境的状态
				time.save();
				//设置粗细和颜色
				time.lineWidth = 6;
				time.strokeStyle = '#000';
				//translate(x,y) 方法重新映射画布上的 (0,0) 位置。
				//x	添加到水平坐标（x）上的值
				// y	添加到垂直坐标（y）上的值
				time.translate(250, 250);
				//rotate() 方法旋转当前的绘图，以弧度计。
				 time.font = "20px Verdana";//必须前置
				 time.fillText('12',-12,-150);
				 time.fillText('3',150,6);
				 time.fillText('6',-6,160);
				 time.fillText('9',-150,6);
				time.rotate((i * 30) * Math.PI / 180);//角度*Math.PI/180=弧度
				time.beginPath();
				time.moveTo(0, -170);
				time.lineTo(0, -190);
				time.closePath();
				time.stroke();
				time.restore();
			}
			/*
			1.save() 方法保存当前图像状态的一份拷贝。
			2.设置刻度的粗细和颜色
			3.利用translate把新的参照坐标设为(250,250)，即下面的坐标就是以(250,250)【圆心】为参照了；所以下面的(0,-170)其实就是(250,70)
			4.旋转30度【360/12=每个小时的度数】
			5.画线，(0, -170)至(0, -190)的线条，其实就是12点的那个刻度，然后通过旋转和循环来画出所有刻度
			6.restore() 方法将绘图状态置为保存值。
			 */
			//时针
				time.save();
				time.lineWidth = 7;
				time.strokeStyle = "black";
				time.translate(250, 250);
				//【360/12】设定指针指向位置
				time.rotate(hour * 30 * Math.PI / 180);
				time.beginPath();
				time.moveTo(0, -140);
				time.lineTo(0, 10);
				time.stroke();
				time.closePath();
				time.restore();
			//分针
				time.save();
				time.lineWidth = 5;
				time.strokeStyle = "black";
				time.translate(250, 250);
				//【360/60】
				time.rotate(min * 6 * Math.PI / 180);
				time.beginPath();
				time.moveTo(0, -160);
				time.lineTo(0, 10);
				time.stroke();
				time.closePath();
				time.restore();
			//秒针
				time.save();
				time.lineWidth = 3;
				time.strokeStyle = "red";
				time.translate(250, 250);
				//【360/60】
				time.rotate(secd * 6 * Math.PI / 180);
				time.beginPath();
				time.moveTo(0, -170);
				time.lineTo(0, 10);
				time.closePath();
				time.stroke();
			//画交叉点
				time.beginPath();
				time.arc(0, 0, 5, 0, 360, false);
				time.closePath();
				time.fillStyle = "gray";
				time.fill();
				time.stroke();
				time.beginPath();
				time.arc(0, -150, 5, 0, 360, false);
				time.closePath();
				time.fillStyle = "gray";
				time.fill();
				time.stroke();
				time.restore();
		}

		clock();
		//每一秒执行一次
		setInterval(clock,1000)
	</script>