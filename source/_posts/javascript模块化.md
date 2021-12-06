---
title: javascript模块化
tags: [javascript]
date: 2016-03-25 15:49:58
description: javascript模块化
categories: [js]
---


模块化是一个通用的编程最佳实践。程序的模块化使我们可以更方便地使用别人的代码，想要什么功能，就加载什么模块，从而提高代码的利用效率，增加开发速度。

模块就像积木，有了它，我们可以搭出各种各种功能样式的程序。积木有什么特点？小而简单。同样的，我们程序中的模块也要做到这一点，确保自己创建的函数一次只完成一个工作，这样其他开发者可以简单地调试与修改你的代码，而不需浏览所有代码才能弄清每一个代码块执行了什么功能。只有做到像这样地小而简单，才能实现其通用功能。

<!-- more -->

## javascript模块化的方法

**函数封装**
JavaScript的作用域就是基于函数的，所以我们可以把函数作为模块。
```js
function fn1(){
    //code
}

function fn2(){
    //code
}
```
缺点："污染"了全局变量，无法保证不与其他模块发生变量名冲突

**对象**
```js
var myModule1 = {
    fn1: function(){
        //code
    },
    fn2: function(){
        //code
    }
}
```
缺点：会暴露所有模块成员，内部状态可以被外部改写

**立即自执行函数——推荐**
```js
var myModule = (function(){
    function fn1(){
        //code
    },
    function fn2(){
        //code
    },
    return {
        fn1: fn1,
        fn2: fn2
    };
})();
```

## 小而简单

关于小而简单，我们看一个例子，比如我们现在想编写一个创建新链接的函数，并且为类型是"mailto"超链接添加一个class。可以这样做：
```js
function addLink(text, url, parentElement) {
    var newLink = document.createElement('a');//创建a标签
    newLink.setAttribute('href', url);//为a标签设置href属性
    newLink.appendChild(document.createTextNode(text));//为a标签添加文本
    if(url.indexOf("mailto:")==-1){
        newLink.className = 'mail';
    }
    parentElement.appendChild(newLink);//将a标签添加到页面
}
```
这样写能够工作，但你或许会发现自己又不得进行其他的功能添加，于是，这个函数又不适用了。所以，函数越特殊，越难以适用于不同情形。
这里的函数写法没有达到模块化的要求——一个函数只干一件事。我们将函数改编下：
```
function createLink(text,url) {
    var newLink = document.createElement('a');
    newLink.setAttribute('href', url);
    newLink.appendChild(document.createTextNode(text));
    return newLink;
}
```
这里createLink函数只做一件事——创建并返回要添加到页面中的a标签（小而简单），这样我们就可以在任何需要创建超链接的情况下调用这样函数。

## CommonJS
在浏览器环境下，没有模块也不是特别大的问题，毕竟网页程序的复杂性有限；但是在服务器端，一定要有模块与操作系统和其他应用程序互动，否则根本没法编程。虽然JavaScript在web端发展这么多年，但是第一个流行的模块化规范却由服务器端的JavaScript应用带来，CommonJS规范是由NodeJS发扬光大，这标志着JavaScript模块化编程正式登上舞台。
node.js的模块系统，就是依据CommonJS规范实现的。在CommonJS中，有一个全局性方法require()，用于加载模块。
加载模块：
```js
var math = require('math');
```
调用模块：
```
  math.add(2,3)
```
CommonJS规范不适用于浏览器环境，因为它存在一个重大的局限，上例中第二行math.add(2, 3)必须要在math.js加载完成后才能运行，而模块都放在服务器端，所以可能要等很长时间，等待时间取决于网速的快慢。

CommonJS规范适用于服务器端，因为对于服务端来说，所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间

## 模块应该怎么定义和怎么加载?

**[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)**
`Asynchronous Module Definition`异步模块定义,主要代表：[require.js](http://requirejs.org/docs/)
目的：
（1）实现js文件的异步加载，避免网页失去响应；
（2）管理模块之间的依赖性，便于代码的编写和维护。
1. 定义模块
```
define(["./cart", "./inventory"], function(cart, inventory) {
        //通过[]引入依赖
        return {
            color: "blue",
            size: "large",
            addToCart: function() {
                inventory.decrement(this);
                cart.add(this);
            }
        }
    }
);
```
2. 加载模块
```
require( ["some/module", "my/module", "a.js", "b.js"],
    function(someModule,    myModule) {
        //This function will be called when all the dependencies
        //listed above are loaded. Note that this function could
        //be called before the page is loaded.
        //This callback is optional.
    }
  );
```

**[CMD](https://github.com/cmdjs/specification/blob/master/draft/module.md)**
`Common Module Definition`通用模块定义，CMD规范是国内发展出来的。主要代表：[sea.js](http://seajs.org/docs/)
1. 定义模块
```
define(function(require, exports, module) {

  // 通过 require 引入依赖
  var $ = require('jquery');
  var Spinning = require('./spinning');

  // 通过 exports 对外提供接口
  exports.doSomething = ...

  // 或者通过 module.exports 提供整个接口
  module.exports = ...

});
```
2. 加载模块
```
seajs.use("../static/hello/src/main")
```

区别：

1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.

2. CMD 推崇依赖就近，AMD 推崇依赖前置。



我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！