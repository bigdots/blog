
## 面向对象

- prototypeObj.isPrototypeOf(object)

    用于测试一个对象是否存在于另一个对象的原型链上

- Object.getPrototypeOf(object)

    返回指定对象的原型

- obj.hasOwnProperty(prop)

    返回一个布尔值，指示对象自身属性中是否具有指定的属性**不会访问到原型上**

- prop in object

    判断指定的属性是否在指定的对象或其原型链中

- object instanceof constructor

    测试一个对象在其原型链中是否存在一个**构造函数的 prototype 属性**。

### 创建对象
1. 工厂模式
2. 构造函数模式
3. 原型模式
4. 构造函数+原型的组合模式
5. 寄生构造函数模式
6. 稳妥构造函数模式


### 继承
1. 原型链
2. 构造函数
3. 组合继承
4. 原型式继承[Object.create()]
5. 寄生式继承
6. 寄生组合式继承


## 严格模式

`use strict`

- 变量必须声明
- 对象不能出现重复属性名
- arguments改变，不会影响函数参数
- eval，arguments变为关键字，不能作为变量名
- 不允许使用with
- 不用call，apply，bind改变this指向，一般函数调用指向undefined

[Javascript 严格模式详解](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode)

## this 机制

new 绑定 > 明确绑定（apply,call,bind） > 隐含绑定(调用者) > 默认绑定(全局对象)

## call 、 apply 和 bind
```js
fn.call(obj , 100 , 200);
fn.apply(obj , [100, 200]);
fn.bind(obj , 100 , 200);fn();
```

## let 和 const
- 在代码块`{}`内有效；
- 不允许重复声明；
- 不存在变量提升；
- 存在暂时性死区；

### let

声明一个局部变量，不用多说。

### const

声明一个常量，但**const 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。**

对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const 只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。

```js
const a = [];
a.push("Hello"); // 可执行
a.length = 0; // 可执行
a = ["Dave"]; // 报错

const b = {};
b.a = 2; // 可执行
b = { a: 2 }; // 报错
```

## ES6 class 和 ES5 构造函数

## 继承

- 利用原型 (包括Object.create)

- 利用call和apply

## AMD、CMD 和 ES6 Module


## 自执行函数

```js

(function($){
    return {
         hello: function(){}
    }
})(Jquery)
```

## CommonJs

产生背景：
node的产生，js走向服务端，需要一个模块系统。

```js
var http = require('http'); //会阻塞

//...
```

**同步／阻塞**执行，时间为I/O的时间。

## AMD 和 CMD

产生背景：想要将模块系统应用于浏览器。**异步模块加载机制**，利用回调。

无争议：浏览器的特性需要js文件提前加载

异议：执行时机


[前端模块化开发那点历史](https://github.com/seajs/seajs/issues/588)

### AMD

因为依赖前置，模块代码会被提前执行，造成性能浪费。

最佳实践：requireJS;




```js
// hello.js
// a,b依赖前置，会提前执行
define(['a', 'b'], function(a, ,b){
    // ...
    
    return{
        hello: function(){}
    }
})
```



### CMD

最佳实践：seajs

就近依赖，按需执行。

```js
// hello.js
define(function(require, exports, module){
    var a = require('a'); // 就近依赖
    // do a

    var b = require( 'b' );
    // do b

    module.exports.hello = hello; // 对外输出hello
})
```


## ES6 模块标准

第一版标准。

`export`

`import`

## 箭头函数

1. 函数体内的 this 对象，它是固定的，就是定义时所在的对象，而不是使用时所在的对象。

2. 不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误。

3. 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

4. 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

**this 指向的固定化，并不是因为箭头函数内部有绑定 this 的机制，实际原因是箭头函数根本没有自己的 this，导致内部的 this 就是外层代码块的 this。正是因为它没有 this，所以也就不能用作构造函数。**

内部实现

```js
// ES6
function foo() {
    setTimeout(() => {
        console.log("id:", this.id);
    }, 100);
}

// ES5
function foo() {
    var _this = this;

    setTimeout(function() {
        console.log("id:", _this.id);
    }, 100);
}
```


## 垃圾回收

- 标记清除：这是js最常用的垃圾回收方法，当一个变量进入执行环境时，例如函数中声明一个变量，将其标记为进入环境，当变量离开环境时，（函数执行结束），标记为离开环境

- 引用计数: 跟踪记录每个值被引用的次数，声明一个变量，并将引用 类型赋值给这个变量，则这个值的引用次数+1，当变量的值变成了另一个，则这个值的引用次数-1，当值的引用次数为0的时候，就回收