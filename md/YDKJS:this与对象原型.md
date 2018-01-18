# this 与对象原型

<!-- TOC -->

- [this 与对象原型](#this-与对象原型)
    - [this 还是 that](#this-还是-that)
        - [为什么要使用 this](#为什么要使用-this)
        - [困惑](#困惑)
        - [什么是 this](#什么是-this)
    - [this 豁然开朗](#this-豁然开朗)
        - [调用点](#调用点)
        - [this 绑定规则](#this-绑定规则)
        - [一切皆有顺序](#一切皆有顺序)
        - [判定 this](#判定-this)
    - [对象](#对象)
        - [语法](#语法)
        - [类型](#类型)
        - [内容](#内容)
        - [遍历](#遍历)
    - [混淆“类”的对象](#混淆类的对象)
    - [原型](#原型)
    - [行为委托](#行为委托)

<!-- /TOC -->

## this 还是 that

> this 是在每个函数作用域中自动定义的特殊标识符关键字。

### 为什么要使用 this

1. 目标：

   允许函数对多个环境对象进行复用，而不是针对不同的环境重复定义。

2. 实现：

   * 明确地将环境对象传递给函数。

   * 通过 this 机制自动引用恰当的执行环境

3. 比较：

   this 机制提供了更优雅的方式来隐含地传递一个对象引用。实现了更加干净的 API 设计和更容易的复用。

### 困惑

开发者在对 this 的理解上往往存在俩种误解：

1. this 是函数自身的引用

   第一种常见的错误倾向是认为 this 指向函数自己。

   ```js
   function foo() {
       this.count++;
   }

   foo.count = 0;

   for (var i = 1; i <= 5; i++) {
       console.log(i); // 1,2,3,4,5
       foo();
   }

   console.log(foo.count); //0
   ```

   上面 foo 被执行了五次，但是 foo.count 的值依然为 0，这说明 this 根本就不指向那个函数对象。

2. this 是函数词法作用域的引用

   第二种常见的对 this 指向的误解是认为它指向当前的函数作用域。这是一种严重的误导，this 不会以任何方式指向函数的词法作用域。

   ```js
   var a = 3;
   function foo() {
       var a = 2;
       console.log(this.a);
   }

   foo(); // 3
   ```

   如果 this 指向的是函数的词法作用域，那么执行 foo 的结果应该输出 2，但是显而易见，输出的并不是 3，这说明**this 和词法作用域之间没有桥**，不能使用 this 在词法作用域中查找东西。

### 什么是 this

**this 实际上是在函数被调用时建立的一个绑定，它指向什么完全是由函数的调用点(call-site)来决定的。**

this 不是编写时绑定而是运行时绑定，它依赖于函数调用的上下文条件。this 的绑定和函数声明的位置无关，反而和函数被调用的方式有关。

当一个函数被调用时，会创建一个执行环境（活动记录），它包括函数是从何处(call-stack)被调用的，函数是如何被调用的，被传递了什么参数等信息。这个记录的属性之一，就是在函数执行期间将被使用的 this 引用。

## this 豁然开朗

this 完全是一个根据**调用点**而为每次函数调用建立的绑定。

### 调用点

> 调用点就是函数在代码中被调用的位置。

要弄明白 this 指向，我们必须先寻找到调用点。

调用栈和调用点：

```js
function foo() {
    // 调用栈： baz -> bar -> foo
    console.log("foo");
}

function bar() {
    // 调用栈： baz -> bar
    console.log("bar");
    foo(); // foo的调用点
}

function baz() {
    // 调用栈： baz
    console.log("baz");
    bar(); // bar的调用点
}

baz(); // baz的调用点
```

**调用点是影响 this 绑定的唯一因素。**

### this 绑定规则

1. 默认绑定

   **独立函数**调用时，适用于这种规则。

   ```js
   var a = 2;
   function foo() {
       console.log(this.a);
   }
   foo(); // 2
   ```

   上述代码，this 指向了全局对象，这是为什么？

   这是因为对于默认绑定来说：如果没有在严格模式下运行，**全局对象是唯一合法的**。

```js
var a = 2;
function foo() {
    "use strict";
    console.log(this.a);
}
foo(); // TypeError: this is undefined
```

严格模式下，必须指明函数的调用者。

2. 隐含绑定

   适用于调用者拥有一个环境对象（也称拥有者对象或容器对象）的情况。

   ```js
   function foo() {
       console.log(this.a);
   }

   var obj = {
       a: 2,
       foo: foo
   };

   obj.foo(); // 2
   ```

   在这里，函数 foo 作为属性被添加到对象 obj 上，可以说这个函数被 obj 所“拥有”或“包含”。在函数 foo 调用的位置上，它被冠以一个指向 obj 的对象引用。

   隐含绑定的规则：当一个函数引用一个环境对象时，这个对象应当被用于函数调用的 this 绑定。

   **隐含丢失**

   ```js
   function foo() {
       console.log(this.a);
   }

   var obj = {
       a: 2,
       foo: foo
   };

   var bar = obj.foo;

   var a = "global";

   bar(); // global
   ```

   尽管 bar 似乎是 obj.foo 的引用，但实际上它只是一个 foo 的 引用而已。另外从调用点看来，是独立函数调用，因此默认绑定规则起了作用。

3. 明确绑定

   1. `call` 和 `apply` 提供了一种直接指明函数 this 的方法。

      ```js
      function foo() {
          console.log(this.a);
      }

      var obj = {
          a: 2
      };

      foo.call(obj); //2
      foo.apply(obj); //2
      ```

      这里使用了`call` 和 `apply`强制函数的 this 指向 obj 。

      如果第参数传递的是简单原始类型，那么这个原始类型会被包装在它的对象类型中。

      `call` 和 `apply` 在绑定 this 的角度上没有任何区别。它们只是在参数的传递上有所区别。

   2. 硬绑定

      明确绑定仍存在一个问题：它还是无法解决隐含丢失。而硬绑定正是为了解决这一问题。

      **ES5 提供了 bind 这一工具用于硬绑定。**

      ```js
      function foo() {
          console.log(this.a);
      }

      var obj = {
          a: 2
      };

      var bar = foo.bind(obj);

      bar(); // 2
      ```

      bind 返回一个硬编码的新函数，它使用你指定的 this 环境来调用原本的函数。

      **硬绑定其实是 明确绑定 的变种。**

      ```js
      // 简单的bind函数
      function bind(fn, obj) {
          return function() {
              return fn.apply(obj);
          };
      }
      ```

4. new 绑定

在 JS 中，构造器仅仅是一个函数，它们被前置的 new 操作符调用。它们不依附于类，也不初始化类。

当函数作为 new 表达式的一部分被调用时，它才是一个构造器：初始化这个新创建的对象。

**所以实际上，不存在“构造函数”这样的东西，而只有函数的构造期调用。**

当 new 表达式调用时：

1. 创建一个全新的对象；
2. 将这个对象接入原型链
3. 将这个对象设置为函数调用的 this 绑定
4. 返回这个对象（需要排除函数返回一个它自己的对象的情况）

```js
function foo(a) {
    this.a = a;
}
var bar = new foo(2);
console.log(bar.a);
```

我们通过 new 来调用 foo，从而创建了一个新的对象，并将这个新对象作为 foo 调用的 this。这种方式实现了 new 绑定。

### 一切皆有顺序

new 绑定 > 明确绑定 > 隐含绑定 > 默认绑定

### 判定 this

通过上面的顺序，我们可以轻易地判定 this 的指向了：

* 如果使用了 new ，那么 this 就是新构建的对象；
* 如果使用了 apply／call 和 bind，this 就是明确制定的对象；
* 如果函数存在调用者，也就是环境对象，那么 this 就是那个环境对象；
* 否则，使用默认规则。严格模式下为 undefinde,否则则是全局对象。

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

Javscript中的一切皆对象的理解是存在错误的。

简单基本类型自身不是 object。typeof null 返回 object 是语言中的一个bug，实际上null是它自己的基本类型。

内建对象

内建对象仅仅只是函数，每一个都可以被用作构造器。

### 内容

1. 可计算属性名

2. 属性与方法

3. 数组

4. 复制对象

5. 属性描述符

6. 不变性

7. [[Get]]

8. [[Put]]

9. Getter 和 Setter

10. 存在性

### 遍历

## 混淆“类”的对象

## 原型

## 行为委托
