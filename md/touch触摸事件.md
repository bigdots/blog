title: touch触摸事件
date: 2016-01-15 15:27:19
tags: [javascript]
description: touch触摸事件

---

## 事件对象
事件对象是用来记录一些事件发生时的相关信息的对象。事件对象只有事件发生时才会产生，并且只能是事件处理函数内部访问，在所有事件处理函数运行结束后，事件对象就被销毁！

+ W3C DOM把事件对象作为事件处理函数的第一个参数传入进去
+ IE将事件对象作为window对象的一个属性（相当于全局变量）

<!-- more -->

## originalEvent对象
在一次偶然的使用中，我发现当使用on()函数并且传入第二个选择器参数时,e.touches[0]的访问为undefined，打印e发现，它的事件对象不是原生的事件对象。经查阅发现它是jquery事件对象。
```javascript
$(window).on("touchstart","body",function(e){
	console.log(e)
})
```
![](/images/201601/touchclass1.png)

上面例子中event中有一个originalEvent属性，而这才是真正的touch事件。jQuery.Event 是一个构造函数，其创建一个可读写的jQuery事件对象，并在event 对象保留了对这个原生事件对象 event 的引用($event.originalEvent)。我们绑定的事件处理程序所处理的事件对象都是 $event。该方法也可以传递一个自定义事件的类型名，用于生成用户自定义事件对象。


## touch事件

`touchmove`: 当手指在屏幕上滑动的时候连续地触发。
`touchstart`: 当手指触摸屏幕时候触发，即使已经有一个手指放在屏幕上也会触发
`touchend`:  当手指从屏幕上离开的时候触发。

## TouchEvent对象
每一个touch事件的触发都会产生一个TouchEvent对象，以下是TouchEvent对象三个比较常用的重要属性

+ touches   当前位于屏幕上的所有手指的一个列表。
+ targetTouches  特定于事件目标的Touch对象的数组。[当前手指]
+ changeTouches  表示自上次触摸以来发生了什么改变的Touch对象的数组。

在这里，我用js写了一个touch事件，点击屏幕可触发,将其事件事件对象在控制台打印出，结果如下（箭头指向的是上述三个属性）：
```javascript
window.addEventListener("touchstart",function(event){
	console.log(event);
})
```
![](/images/201601/touch.png)
## 触摸事件对象属性
touches、targetTou、changeTouches都包含以下属性值

+ clientX：触摸目标在视口中的x坐标。
+ clientY：触摸目标在视口中的y坐标。
+ identifier：标识触摸的唯一ID。
+ pageX：触摸目标在页面中的x坐标。
+ pageY：触摸目标在页面中的y坐标。
+ screenX：触摸目标在屏幕中的x坐标。
+ screenY：触摸目标在屏幕中的y坐标。
+ target：触摸的DOM节点目标。

还是上面的那个例子，changeTouches对象在控制台输出如下：
![](/images/201601/touchclass.png)