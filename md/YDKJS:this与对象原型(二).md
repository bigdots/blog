# YDKJS:this与对象原型(二)

<!-- TOC -->

- [YDKJS:this与对象原型(二)](#ydkjsthis与对象原型二)
    - [对象](#对象)
        - [语法](#语法)
        - [类型](#类型)
        - [内容](#内容)
        - [计算型属性名](#计算型属性名)
        - [属性(Property) vs 方法(Method)](#属性property-vs-方法method)
        - [数组](#数组)
        - [复制对象](#复制对象)
        - [属性描述符](#属性描述符)
        - [实现属性／对象的不可变性](#实现属性／对象的不可变性)
        - [[[Get]]](#get)
        - [[[Put]]](#put)
        - [Getter 和 Setter](#getter-和-setter)
        - [存在性](#存在性)
        - [遍历](#遍历)
    - [混淆“类”的对象](#混淆类的对象)

<!-- /TOC -->

## 对象

什么是对象？this 为什么需要指向对象？

### 语法

对象主要来源于俩种形式：

1. 字面量形式

    ```js
    var myObj = {};
    ```

2. 构造形式

    ```js
    var myObj = new Object();
    ```

### 类型

Javscript 中的一切皆对象的理解是存在错误的。

简单基本类型自身不是 object。typeof null 返回 object 是语言中的一个 bug，实际上 null 是它自己的基本类型。

**内建对象**

* String
* Number
* Boolean
* Object
* Function
* Array
* Date
* RegExp
* Error

内建对象仅仅只是函数，每一个都可以被用作构造器。new 一个内建对象的结果是一个新构建的相应**子类型的对象**。

```js
var strObj = new String("I am a string");
Object.prototype.toString.call(strObj); // [object String]
```

`Object.prototype.toString`方法可以考察自类型的内部。

**内建对象与基本类型的关系？**

基本类型不是一个对象，它是一个不可变的基本字面值。为了对它进行操作都需要一个相应的对象。

在 js 中，必要的时候会将基本类型值转化为相应的对象类型，比如：

```js
var str = "I am a string";
console.log(str.length); // 13
```

这里调用`str.length`，js 会先将 str 转化成 `new String('I am a string')`，再进行属性读取。

**特殊： null 和 undefined 没有基本包装类型。Date 没有字面量值。**

**注意：仅在必要时使用构建形式，推荐使用字面形式**

### 内容

对象的内容是由属性构成的，属性就是储存在特定位置上（任意类型）的值。

引擎会根据自己的实现来存储属性值，而且通常都不是把它们存储在对象内部。储存在对象内部的是这些对象的名称，它们像指针一样指向值存储的地方。

**访问方式：**

* 属性（property）访问

    使用 `.` 操作符。后面接一个`标识符`。

* 键（key）访问

    使用 `[]` 操作符。可以接受任何兼容`UTF-8/unicode`的字符串。可以使用动态键（表达式）。

属性名总是字符串。使用字符串以外的值，它会首先被转化为字符串。

```js
var myObj = {};
myObj[true] = "foo";
myObj[3] = "bar";
myObj[myObj] = "baz";

myObj["true"]; // foo
myObj["3"]; // bar
myObj["[object Object]"]; //baz
```

### 计算型属性名

`[]`提供了使用表达式作为键名的方法。但这种方式并不支持使用字面量语法。

ES6 加入了计算型属性名，可以在字面量声明时通过`[]`指定表达式属性。

```js
var prefix = "foo";

var myObj = {
    [prefix + "bar"]: "hello"
};
myObj["foobar"]; // hello
```

### 属性(Property) vs 方法(Method)

人们喜欢将属性进行区分，把属于对象（类）的函数称为方法。但从技术上讲，函数不会属于对象 ，因为 this 是在运行时动态绑定的，说明它与对象的关系至多是间接的。

```js
function foo() {
    console.log("foo");
}

var someFoo = foo;

var myObj = {
    someFoo: foo
};

foo; // ƒ foo(){...}
someFoo; // ƒ foo(){...}
myObj.someFoo; // ƒ foo(){...}
```

`myObj.someFoo` 和 `someFoo` 都是对 foo 函数的分离引用，它们并不意味着很特别或者被某个对象所拥有。

### 数组

跟对象采用键来存储值一样，数组采用数字索引（数字小标）来存储值。

数组的索引都是非负整数。

如果在数组上添加一个似数字属性，那么它会称为一个数组索引。

```js
var foo = ["foo", 123];
foo["3"] = "bar";
foo[3]; // bar
```

### 复制对象

浅拷贝 和 深拷贝。

使用 JSON 安全对象来进行神拷贝。

### 属性描述符

自 ES5 开始，js 添加了属性描述符来考察或者描述属性。

```js
var obj = {
    a: 2
};
Object.getOwnPropertyDescriptor(obj, "a");

// {value: 2, writable: true, enumerable: true, configurable: true}
```

属性描述符：

* writable

    可写性。设置为 false 则不可写。

* enumerable

    可枚举性。它控制对象属性能否被枚举，比如 for...in 循环。

* configurable

    可配置性。将它设为 false：

    * defineProperty 也不可再使用，会报错。（说明是单向操作，不可撤销）
    * delete 操作失效

我们可以使用`Object.defineProperty`来添加和修改属性。

### 实现属性／对象的不可变性

1. 对象常量

    通过 `writable: false` 和 `configurable: false`可以实现常量的创建。不可改变，重定义或者删除。

2. 防止扩展

    通过 `Object.preventExtensions()` 方法可以防止一个对象添加新属性。

    ```js
    var obj = { a: 1 };
    Object.preventExtensions(obj);
    obj.b = 2;
    obj.b; //undefined
    ```

3. 封印

    `Object.seal()`创建一个封印的对象。它既防扩展又不可配置。相等于同事调用`Object.preventExtensions()`和设置`configurable: false`。

4. 冻结

    `Object.freeze()`创建一个冻结对象。它相当于`Object.seal()` + `writable: false`。这事目前最高级别的不可变性。

### [[Get]]

属性访问实际上执行了一个 `[[Get]]` 操作。它会首先检查对象，寻找一个拥有被请求的名称的属性，找到，则返回相应的值。

1. 找到，返回相应的值。
2. 未找到，遍历链 返回 `undefined`。

### [[Put]]

相应于 `[[Get]]` 操作，js有个相应的 `[[Put]]` 操作用于对象属性赋值。

1. 这个属性是访问器描述符吗？
2. 是`writable: false`的数据描述符吗？
3. 否则。

### Getter 和 Setter

### 存在性

### 遍历

## 混淆“类”的对象
