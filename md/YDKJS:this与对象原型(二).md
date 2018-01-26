# YDKJS:this 与对象原型(二)

<!-- TOC -->

- [YDKJS:this 与对象原型(二)](#ydkjsthis-与对象原型二)
    - [语法](#语法)
    - [类型](#类型)
    - [内容](#内容)
        - [计算型属性名](#计算型属性名)
        - [属性(Property) vs 方法(Method)](#属性property-vs-方法method)
        - [数组](#数组)
        - [复制对象](#复制对象)
        - [属性描述符](#属性描述符)
        - [不变性](#不变性)
        - [[[Get]]](#get)
        - [[[Put]]](#put)
        - [Getter 和 Setter](#getter-和-setter)
        - [存在性](#存在性)
    - [遍历](#遍历)

<!-- /TOC -->

什么是对象？this 为什么需要指向对象？

## 语法

对象主要来源于俩种形式：

1. 字面量形式

    ```js
    var myObj = {};
    ```

2. 构造形式

    ```js
    var myObj = new Object();
    ```

## 类型

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

## 内容

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

    ```js
    var myObject = {};
    Object.defineProperty(
        myObject,
        "a",
        // 让 a 像普通属性一样可以枚举
        { enumerable: true, value: 2 }
    );
    Object.defineProperty(
        myObject,
        "b",
        // 让 b 不可枚举
        { enumerable: false, value: 3 }
    );

    for (var k in myObject) {
        console.log(k, myObject[k]);
    }
    // a 2
    ```

    可以看到，myObject.b 不会出现在 for..in 循环中。

    其他检验属性是否可枚举的方法：

    1. `propertyIsEnumerable(..)` 会检查给定的属性名是否直接存在于对象中(而不是在原型链 上)并且满足 `enumerable:true`。

        ```js
        myObject.propertyIsEnumerable("a"); //true
        myObject.propertyIsEnumerable("b"); // false
        ```

    2. `Object.keys(..)` 会返回一个数组，包含所有可枚举属性，`Object.getOwnPropertyNames(..)` 会返回一个数组，包含所有属性，无论它们是否可枚举。

        ```js
        Object.keys(myObject); // ["a"]
        Object.getOwnPropertyNames(myObject); //  ["a", "b"]
        ```

- configurable

    可配置性。将它设为 false：

    * defineProperty 也不可再使用，会报错。（说明是单向操作，不可撤销）
    * delete 操作失效

我们可以使用`Object.defineProperty`来添加和修改属性。

### 不变性

如何实现属性/对象的不可变性？

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

属性访问实际上执行了一个 `[[Get]]` 操作。它会首先检查对象，查找是否有名称相同的属性：

1. 存在，返回相应的值。
2. 不存在，返回 `undefined`。（这个查找会遍历原型链）

```js
var myObject = {
    a: 2
};
myObject.a; // 2
myObject.b; // undefined
```


### [[Put]]

相应于 `[[Get]]` 操作，js 有个相应的 `[[Put]]` 操作。

[[Put]] 操作的实际行为取决于许多因素，包括对象中是否已经存在这个属性(这是最重要的因素)。

如果已经存在这个属性：

1. 属性是否是访问描述符(参见 Getter 和 Setter)?如果是并且存在 setter 就调用 setter。
2. 属性的数据描述符中 writable 是否是 false?如果是，在非严格模式下静默失败，在严格模式下抛出 TypeError 异常。
3. 如果都不是，将该值设置为属性的值。

如果不存在，更复杂，详见[原型]()

### Getter 和 Setter

对象默认的 [[Put]] 和 [[Get]] 操作分别可以控制属性值的设置和获取。

在 ES5 中可以使用 getter 和 setter 部分改写默认操作，但是只能应用在单个属性上，无法应用在整个对象上。

getter 是一个隐藏函数，会在获取属性值时调用。setter 也是一个隐藏函数，会在设置属性值时调用。

当你给一个属性定义 getter、setter 或者两者都有时，这个属性会被定义为“访问描述符”

JavaScript 会忽略访问描述符的 value 和 writable 特性，取而代之的是关心 set 和 get(还有 configurable 和 enumerable)特性。

```js
var myObject = {
    // 给 a 定义一个 getter
    get a() {
        return 2;
    }
};

Object.defineProperty(
    myObject, // 目标对象 "b", // 属性名
    "b",
    {
        // 给 b 设置一个 getter
        get: function() {
            return this.a * 2;
        },
        // 确保 b 会出现在对象的属性列表中
        enumerable: true
    }
);

myObject.a; // 2
myObject.b; // 4
```

`get a() { .. }`和`defineProperty(..)`都会在对象中创建一个不包含值的属性, 对于这个属性的访问会自动调用一个隐藏函数，它的返回值会被当作属性访问的返回值。

通常来说 getter 和 setter 是成对出现的(只定义一个的话 通常会产生意料之外的行为):

```js
var myObject = {
    // 给 a 定义一个 getter
    get a() {
        return this._a_;
    },
    // 给 a 定义一个 setter
    set a(val) {
        this._a_ = val * 2;
    }
};
myObject.a = 2;
myObject.a; // 4
```

### 存在性

```js
var myObject = {
    a: undefined
};
myObject.a; //undefined
myObject.b; //undefined
```

一个属性值有可能 是属性中存储的 undefined，也可能是因为属性不存在所以返回 undefined。那么如何区分 这两种情况呢?

1. in
   in 操作符会检查**属性名**在对象及其 [[Prototype]] 原型链中是否存在

    ```js
    "a" in myObject; //true
    "b" in myObject; //false
    ```

2. hasOwnProperty
   只会检查属性是否在 myObject 对象中，不会检查 [[Prototype]] 链。

    ```js
    myObject.hasOwnProperty("a"); //true
    myObject.hasOwnProperty("b"); //false
    ```

## 遍历

1. for..in
2. for 循环
3. forEach(..)、every(..) 和 some(..)
4. for..of
