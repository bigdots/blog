---
title: javascript中的prototype和constructor
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

## 构造函数 ##
我们知道，ECMAScript5中的Object、Array、Date、RegExp、Function等引用类型都是基于构造函数的,他们本身就是ECMAScript5原生的构造函数。比如，我们可以这样申明一个对象：

	var person = new Object();
这里的Object类型的构造函数就是一个名为Object的构造函数，然后通过new来实例化这个构造函数，从而获得一个新的实例对象。同理，Array、Date等其他类型也是一样。

我们又知道，原型对象包含了特定类型的所有实例共享的属性和方法，这样一来，对于toString()、toValue()这些函数是不是也豁然开朗了，这些函数就是定义在上述特定类型的原型对象上的，所以它们的每一个实例都可以访问。

## prototype和constructor ##

1. prototype

	每一个函数都包含prototype属性，这个属性是一个指针，指向原型对象()。对于引用类型而言，prototype 是保存它们所有实例方法的真正所在。


2. constructor

	每一个object对象都包含有constructor属性，该属性保存着用于创建当前对象的函数。

> 构造函数每个实例的内部将包含一个指针（内部属性），指向构造函数的原型对象。ECMA-262 第5 版中管这个指针叫[[Prototype]]。虽然在脚本中没有标准的方式访问[[Prototype]]，但Firefox、Safari 和Chrome 在每个对象上都支持一个属性__proto__

看了几遍还是觉得绕，还是画个图看下：

![](http://images.cnblogs.com/cnblogs_com/yzg1/855692/o_prototype_constructor.png)