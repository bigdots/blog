---
title: javascript作用域链学习笔记
date: 2016-03-14 14:54:41
tags: [javascript]
description: javascript作用域链
categories: [js]
---

我的博客地址:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

## 作用域链
1. ”JavaScript中的函数运行在它们被定义的作用域里,而不是它们被执行的作用域里.”　——权威指南

2. 在JavaScript中，一切皆对象，包括函数。函数对象和其它对象一样，拥有可以通过代码访问的属性和一系列仅供JavaScript引擎访问的内部属性。其中一个内部属性是[[Scope]]，由ECMA-262标准第三版定义，该内部属性包含了函数被创建的作用域中对象的集合，这个集合被称为函数的作用域链，它决定了哪些数据能被函数访问。

3. 在一个函数被定义的时候, 会将它定义时刻的scope chain链接到这个函数对象的[[scope]]属性.

4. 在一个函数对象被调用的时候，会创建一个活动对象(也就是一个对象), 该对象包含了函数的所有局部变量、命名参数、参数集合以及this，然后此对象会被推入作用域链的前端，当运行期上下文被销毁，活动对象也随之销毁。

5. 在每次调用一个函数的时候 ，就会进入一个函数内的作用域，当从函数返回以后，就返回调用前的作用域.

<!-- more -->

## 实例解析
```javascript
var sayHello = function(l,s){
    var word = "hello world";
}

sayHello();
```

+ 在执行sayHello定义语句的时候, 会创建一个这个函数对象的[[scope]]属性。

+ 将这个[[scope]]属性, 链接到定义它的作用域链上。此时因为func定义在全局环境, 所以此时的[[scope]]只是指向全局活动对象window active object.

+ 在调用sayHello的时候, 会创建一个活动对象(假设为fObj)，并创建arguments属性。然后会给这个对象添加俩个命名属性fObj.l, fObj.s; 对于每一个在这个函数中申明的局部变量和函数定义, 都作为该活动对象的同名命名属性。对于局部变量,变量的值会在真正执行的时候才计算, 此时只是简单的赋为undefined.

+ 将调用参数赋值给形参，对于缺少的调用参数，赋值为undefined。

+ 将这个活动对象做为scope chain的最前端, 并将func的[[scope]]属性所指向的,定义sayHello时候的顶级活动对象, 加入到scope chain.

+ 在发生标识符解析的时候, 就会逆向查询当前scope chain列表的每一个活动对象的属性，如果找到同名的就返回。找不到，那就是这个标识符没有被定义。


作用域链全过程解析：
```javascript
function factory() {
     var name = 'laruence';
     var intro = function(){
          alert('I am ' + name);
     }
     return intro;
}

function app(para){
     var name = para;
     var func = factory();
     func();
}

app('eve');
```

首先当调用app的时候, scope chain是由: {window活动对象(全局)}->{app的活动对象} 组成。此时的scope chain如下:（对于局部变量,变量的值会在真正执行的时候才计算, 此时只是简单的赋为undefined.
）
```
[[scope chain]] = [
{
     para : 'eve',
     name : undefined,
     func : undefined,
     arguments : []
}, {
     window call object
}
]
```

当调用进入factory的函数体的时候, 此时的factory的scope chain为:
```
[[scope chain]] = [
{
     name : undefined,
     intor : undefined
}, {
     window call object
}
]
```

注意: 此时的作用域链中, 并不包含app的活动对象.(JavaScript中的函数运行在它们被定义的作用域里,而不是它们被执行的作用域里.)

在定义intro函数的时候, intro函数的[[scope]]为:
```
[[scope chain]] = [
{
     name : 'laruence',
     intor : undefined
}, {
     window call object
}
]
```

从factory函数返回以后,在app体内调用intor的时候, 发生了标识符解析, 而此时的sope chain是:
```
[[scope chain]] = [
{
     intro call object
}, {
     name : 'laruence',
     intor : undefined
}, {
     window call object
}
]
```
所以, name标识符解析的结果(在上面的作用域链中一层层往上匹配)应该是factory活动对象中的name属性, 也就是’laruence’。


标识符解析过程：
该过程从作用域链头部，也就是从活动对象开始搜索，查找同名的标识符，如果找到了就使用这个标识符对应的变量，如果没找到继续搜索作用域链中的下一个对象，如果搜索完所有对象都未找到，则认为该标识符未定义。函数执行过程中，每个标识符都要经历这样的搜索过程。



## 利用作用域链的代码优化
1. 把全局变量存储到局部变量里再使用

从作用域链的结构可以看出，在运行期上下文的作用域链中，标识符所在的位置越深，读写速度就会越慢。全局变量总是存在于运行期上下文作用域链的最末端，因此在标识符解析的时候，查找全局变量是最慢的。所以，在编写代码的时候应尽量少使用全局变量，尽可能使用局部变量。一个好的经验法则是：如果一个跨作用域的对象被引用了一次以上，则先把它存储到局部变量里再使用。例如下面的代码：

```
function changeColor(){
    var doc=document;
    doc.getElementById("btnChange").onclick=function(){
        doc.getElementById("targetCanvas").style.backgroundColor="red";
    };
}

```



2. 避免改变作用域链
函数每次执行时对应的运行期上下文都是独一无二的，所以多次调用同一个函数就会导致创建多个运行期上下文，当函数执行完毕，执行上下文会被销毁。每一个运行期上下文都和一个作用域链关联。一般情况下，在运行期上下文运行的过程中，其作用域链只会被 with 语句和 catch 语句影响。

```javascript
function initUI(){
    with(document){
        var bd=body,
            links=getElementsByTagName("a"),
            i=0,
            len=links.length;
        while(i < len){
            update(links[i++]);
        }
        getElementById("btnInit").onclick=function(){
            doSomething();
        };
    }
}
```
`with 语句的作用是将代码的作用域设置到一个特定的对象中`

当代码运行到with语句时，运行期上下文的作用域链临时被改变了。一个新的可变对象被创建，它包含了参数指定的对象的所有属性。这个对象将被推入作用域链的头部，这意味着函数的所有局部变量现在处于第二个作用域链对象中，因此访问代价更高了。

参考文章:[Javascript作用域原理](http://www.laruence.com/2009/05/28/863.html)、[理解 JavaScript 作用域和作用域链](http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html)