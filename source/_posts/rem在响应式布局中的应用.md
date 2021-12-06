---
title: rem在响应式布局中的应用
tags: [web]
date: 2016-03-30 10:33:44
description: rem在响应式布局中的应用
categories: [js]
---

## rem/em/px/pt的基友关系
**px**
像素相对长度单位,相对于显示器屏幕分辨率而言

<!-- more -->

**em**
相对长度单位,根据其父元素来设置字体大小

**pt**
point，是印刷行业常用单位，等于1/72英寸

**rem**
CSS3新增的一个相对单位,是根据网页的跟元素（html）来设置字体大小


## rem应用于适配
rem的特性同样适用于width和height，我们可以根据根元素的font-size值来改变元素的宽高值，由此我们应该可以联想到我们可以根据屏幕大小动态地给html设定不同的值，从而达到我们css样式中的适配效果。

## rem的适配规则

**1.选择基准**
虽然我们所写出的页面要在不同的屏幕大小设备上运行，但是我们写页面的时候，必须要选择其中一种屏幕大小作为初始的基准，而这个基准的选择应该根据我们所拿到的视觉稿来决定，

**2.rem数值计算**
正常情况下rem的值默认为16px，这样在整个页面的css计算过程中太过繁琐。比如，现在有个30px宽度的元素，就得写成30/16rem。对于整个页面来说工作量还是挺大的。所以这里提供了俩种方法

+ 可以将html的font-size设置成100px
这样设置，在写单位时直接将数值除以100在加上rem的单位就可以了。如果设计稿的字体是16px；我们就可以写成1.6rem。
    + **这里为什么不用10？**
    因为google等浏览器对最小字体有限制，即最小为12px，所以设置10px会有问题。

+ 使用sass
```sass
$rem : 16x;
@function px_rem($px){
    @return ($px/$rem) + rem;
}
```

**3.动态设置html的font-size**
随着屏幕大小的改变，html的font-size的值应该是`基准rem*改变后的屏幕宽度 / 基准屏幕宽度`


+ 利用css的media query来设置（这种是一个宽度区间内是一个rem）
```
@media (min-device-width : 375px) and (max-device-width : 667px) and (-webkit-min-device-pixel-ratio : 2){
      $rem : 16x;
}
```
+ 利用javascript来动态设置（这种方法每一个宽度点都会有一个新的rem）
```js
document.getElementsByTagName('html')[0].style.fontSize = 基准rem*window.innerWidth / 基准屏幕宽度 + 'px';
```

## 考虑dpr
一般我们获取到的视觉稿大部分尺寸是双倍大小的，我们一般会自觉的将标注/2，但是当我们配合rem使用时，完全可以按照视觉稿上的尺寸来设置。

+ 设计给的稿子双倍的原因是iphone等高清屏手机的存在，高清屏的像素比(device pixel ratio)dpr比较大，所以显示的像素较为清晰。

+ 一般手机的dpr是1，iphone4，iphone5这种高清屏是2，iphone6s plus这种高清屏是3，可以通过js的window.devicePixelRatio获取到当前设备的dpr，所以iphone6给的视觉稿大小是（*2）750×1334了。

+ 拿到了dpr之后，我们就可以在viewport meta头里，取消让浏览器自动缩放页面，而自己去设置viewport的content
```js
meta.setAttribute('content', 'initial-scale=' + 1/dpr + ', maximum-scale=' + 1/dpr + ', minimum-scale=' + 1/dpr + ', user-scalable=no');
```
这样我们就直接可以使用视觉稿上的尺寸了。

[点击查看示例>>](http://bigdots.github.io/blogSource/example/rem.html)

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！