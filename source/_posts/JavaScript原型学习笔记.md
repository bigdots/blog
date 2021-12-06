---
title: JavaScript原型学习笔记
date: 2016-03-15 10:49:29
tags: [javascript]
description:
categories: [js]
---

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

## 原型对象

任何一个对象都有一个prototype的属性，在js中可以把它记为：`__proto__`。 `__proto__`相当于指针,每当你去定义一个prototype的时候，相当于把该实例的`__proto__`指向一个结构体，那么这个被指向结构体就称为该实例的原型。


> 对象是属性的集合，并带有一个单一的原型对象。原型可以是一个对象或空值。

<!-- more -->
如图：下面的例子foo对象有俩个显性的x、y属性并且有一个隐形的prototype属性。
```javascript
var foo = {
    x:10,
    y:20
}
```
![](/images/201601/basic-object.png)


## 原型链

原型对象也是一个简单的对象，也拥有自己的原型。如果有一个非空引用原型的原型对象，这是所谓的原型链。

> 原型链是一个对象的有限链，它用来实现继承和共享。ECMAScript 实现继承的方式主要是依靠原型链。

显然，对于一个好的设计模式，我们可以重用类似的功能/代码，而不需要在每一个对象中重复。因此，下面的代码中，a对象存储b、c这两个对象的共同部分。而b、c只储存他们自己的附加属性或方法。


```javascript
var a = {
  x: 10,
  calculate: function (z) {
    return this.x + this.y + z;
  }
};

var b = {
  y: 20,
  __proto__: a
};

var c = {
  y: 30,
  __proto__: a
};

// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80
```

我们可以看到，b和c有访问定义在a对象的`calculate`方法的入口，这是通过原型链实现的。


+ 属性查找规则：当查找一个对象的属性时，如果一个属性或一个方法在对象本身内没有找到（即对象没有这样一个自己的属性），JavaScript 会向上遍历原型链，直到找到给定名称的属性为止，到查找到达原型链的顶部 - 也就是 Object.prototype - 但是仍然没有找到指定的属性，就会返回 `undefined`，

+ hasOwnProperty是Object.prototype的一个方法，他能判断一个对象是否包含自定义属性而不是原型链上的属性，因为hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。

+ 注意:在使用继承方法时，`this`的值被设置为原始对象，而不是在该方法被发现的（原型）对象中。例如，在上面的例子中，`this.y`是取自于b和c，而不是从a，但是，`this.x`是取自于a，这是通过原型链机制获取的。

+ 事实上，所有引用类型默认都继承了Object，而这个继承也是通过原型链实现的。大家要记住，所有函数的默认原型都是Object 的实例，因此默认原型都会包含一个内部指针，指向Object.prototype。这也正是所有自定义类型都会继承toString()、valueOf()等默认方法的根本原因。object.prototype本身也有`__proto__`，这是一个链的最后一环，被设置为null。所以，我们说上面例子展示的原型链中还应该包括另外一个继承层
次。
![](/images/201601/prototype-chain.png)


## 构造函数和原型

> 每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。


构造函数能做另一个有用的事情-它会自动设置一个新创建的对象的原型对象。这个原型对象存储在`constructorfunction.prototype`属性中。

```javascript
// a constructor function
function Foo(y) {
  // which may create objects
  // by specified pattern: they have after
  // creation own "y" property
  this.y = y;
}

// also "Foo.prototype" stores reference
// to the prototype of newly created objects,
// so we may use it to define shared/inherited
// properties or methods, so the same as in
// previous example we have:

// inherited property "x"
Foo.prototype.x = 10;

// and inherited method "calculate"
Foo.prototype.calculate = function (z) {
  return this.x + this.y + z;
};

// now create our "b" and "c"
// objects using "pattern" Foo
var b = new Foo(20);
var c = new Foo(30);

// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80

// let's show that we reference
// properties we expect

console.log(

  b.__proto__ === Foo.prototype, // true
  c.__proto__ === Foo.prototype, // true

  // also "Foo.prototype" automatically creates
  // a special property "constructor", which is a
  // reference to the constructor function itself;
  // instances "b" and "c" may found it via
  // delegation and use to check their constructor

  b.constructor === Foo, // true
  c.constructor === Foo, // true
  Foo.prototype.constructor === Foo, // true

  b.calculate === b.__proto__.calculate, // true
  b.__proto__.calculate === Foo.prototype.calculate // true

);
```

![](/images/201601/constructor-proto-chain.png)

构造函数Foo也有自己的`__proto__`，它指向function.prototype。foo.prototype是Foo指向B和C原型的显式属性。

## 原型使用方式

1. 对象字面量
```javascript
var Calculator = function (decimalDigits, tax) {
    this.decimalDigits = decimalDigits;
    this.tax = tax;
};
Calculator.prototype = {
    add: function (x, y) {
        return x + y;
    },
    subtract: function (x, y) {
        return x - y;
    }
};
//alert((new Calculator()).add(1, 3));
```

2. 自执行函数
```javascript
var Calculator = function (decimalDigits, tax) {
    this.decimalDigits = decimalDigits;
    this.tax = tax;
};
Calculator.prototype = function () {
    add = function (x, y) {
        return x + y;
    },
    subtract = function (x, y) {
        return x - y;
    }
    return {
        add: add,
        //subtract: subtract
    }
} ();
alert((new Calculator()).add(11, 3)); //14
//alert((new Calculator()).subtract(11, 3)); //not a function
```

这种方法的好处就是可以封装私有的function，通过return的形式暴露出简单的使用名称，以达到public/private的效果。


参考文章:[《ECMA-262》](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)、[《JavaScript探秘：强大的原型和原型链》](http://www.nowamagic.net/librarys/veda/detail/1648)