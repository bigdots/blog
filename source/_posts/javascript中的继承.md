---
title: javascript中的继承
tags: [javascript]
date: 2016-03-18 10:36:42
description:
categories: [js]
---

继承有什么好处？很简单，继承了你爸的财产，自己就可以少奋斗点嘛。开玩笑，言归正传，继承使子类拥有超类的作用域、属性与方法，可以节省程序设计的时间。ECMAScript实现继承主要方式是依靠原型链。

<!-- more -->

## 原型链方式——不建议使用

```js
function SuperType(){
    this.colors = ["red", "blue", "green"];
}
function SubType(){
}
//利用原型链继承
SubType.prototype = new SuperType();
var instance1 = new SubType();
console.log(instance1.colors) //["red", "blue", "green"]
```

上例中，通过`SubType.prototype = new SuperType()`使SubType的原型链上增加了SuperType()这一环。从而使SubType 通过原型链继承了SuperType的`color`属性。

**存在的问题：**

1. 实例共享
在上面的例子中加入以下代码，发现SubType()的实例1对`color`属性做出改变后，实例2获取到的`color`属性是改变后的值。这是因为：
 >每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。
```
instance1.colors.push("black");
console.log(instance1.colors); //["red", "blue", "green", "black", "black"]
var instance2 = new SubType();
console.log(instance2.colors);//["red", "blue", "green", "black", "black"]
```
 + 首先，instance1和instance2都是SubType()的实例，这俩个实例都会包含一个指向原型对象的内部指针
 + SubType()的原型指向SuperType()的一个实例，而且这个实例指向的是SuperType()的原型对象
 + SuperType()的原型对象又指向SuperType()。最终，instance1.colors和instance2.colors的指向都是SuperType()的colors。
2. 没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数，原理同上。


## 构造函数方式——不建议使用

```
function SuperType(){
    this.colors = ["red", "blue", "green"];
}
function SubType(){
    //继承了SuperType
    SuperType.call(this);
}
var instance1 = new SubType();
console.log(instance1.color)//["red", "blue", "green"]
```
> 基本思想即在子类型构造函数的内部调用超类型构造函数。

**call和apply**
全局函数apply和call可以用来改变函数中this的指向。


**存在的问题：**

1. 子类型无法继承超类原型链，导致所有类型都只能使用构造函数模式；
2. 方法都在构造函数中定义，函数无法复用。


## 组合继承——最常用的继承模式

```
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
    console.log(this.name);
};
function SubType(name, age){
    //继承属性
    SuperType.call(this, name);   //第二次调用SuperType()
    this.age = age;
}
//继承方法
SubType.prototype = new SuperType(); //第一次调用SuperType()
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function(){
    console.log(this.age);
};
var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
console.log(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29
var instance2 = new SubType("Greg", 27);
console.log(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge();
```
基本思想：
 + 通过原型链来继承超类的sayName()方法；
 + 通过构造函数的方式来使SuperType()里的属性私有化；

为什么可以实现属性私有化？
 + 第一次调用超类，SubType.prototype 会得到两个属性：name 和colors；它们都是SuperType 的实例属性，位于SubType 的原型中；
 + 第二次调用超类，在新对象上创建了实例属性name 和colors。于是，这两个属性就屏蔽了原型中的两个同名属性；

**存在的问题：**
无论什么情况下，都会调用两次超类型构造函数:：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部


## 原型式继承
```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = Object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");
var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
console.log(person.friends); //["Shelby", "Court", "Van", "Rob", "Barbie"]
```
*ECMAScript 5 通过新增Object.create()方法规范化了原型式继承。这个方法接收两个参数：一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象*

+ 将person对象作为基础对象
+ 把person对象传入到object()函数中，然后该函数就会返回一个新对象F()，这个新对象将person 作为原型
+ 适用于让一个对象与另一个对象保持类似的情况

**存在同原型链方式一样的问题**

## 寄生式基础
```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
function createAnother(original){
    var clone = object(original); //通过调用函数创建一个新对象
    clone.sayHi = function(){ //以某种方式来增强这个对象
    console.log("hi");
};
return clone; //返回这个对象
}
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"
```

+ 将person对象作为基础对象
+ 将基础对象传递给object()函数，将返回的新对象赋值给clone；（示范继承模式时使用的object()函数不是必需的；任何能够返回新对象的函数都适用于此模式。）
+ 为clone 对象添加一个新方法sayHi()，最后返回clone 对象

## 寄生组合式继承——被认为是引用类型最理想的继承范式

```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype); //创建对象
    prototype.constructor = subType; //增强对象
    subType.prototype = prototype; //指定对象
}

function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
    console.log(this.name);
};
function SubType(name, age){
    SuperType.call(this, name); //调用一次SuperType()
    this.age = age;
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function(){
    console.log(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.sayName();
console.log(instance1.colors);
```

+ 将超类的原型对位基础对象，并且传递给object()函数，返回新对象赋值给prototype；
+ 将子类的原型指向prototype，即超类的原型，这里继承了超类的sayName方法
+ 子类中利用构造方法使子类上创建了name和colors属性

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

参考书籍：[《javascript高级程序设计》](#)