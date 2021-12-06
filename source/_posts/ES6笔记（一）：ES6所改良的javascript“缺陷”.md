---
title: ES6笔记（一）：ES6所改良的javascript“缺陷”
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

## 块级作用域 ##

ES5没有块级作用域，只有全局作用域和函数作用域，由于这一点，变量的作用域甚广，所以一进入函数就要马上将它创建出来。这就造成了所谓的变量提升。

ES5的“变量提升”这一特性往往一不小心就会造成一下错误：

1. 内层变量覆盖外层变量

		var tmp = new Date();
		function f() {
		  console.log(tmp);
		  if (false) {    //执行则undefined
		    var tmp = "hello world";
		  }
		}

2. 变量泄露，成为全局变量

		var s = 'hello';
		for (var i = 0; i < s.length; i++) {
		  console.log(s[i]);
		}
		console.log(i); // 5

往常我们往往是使用`闭包`来解决这一问题的（比如自执行函数）。现在，基于这一问题，ES6增加了`块级作用域`,所以不再需要自执行函数了。

### let 和 const ###

ES6是是向后兼容的，而保持向后兼容性意味着永不改变JS代码在Web平台上的行为，所以`var`创建的变量其作用域依旧将会是全局作用域和函数作用域。这样以来，即使拥有了块级作用域，也无法解决ES5的“变量提升”问题。所以，这里ES6新增了俩个新关键词：`let`和`const`。

1. let
  
	“let是更完美的var”，它有着更好的作用域规则。

2. const
	const声明一个只读的常量。一旦声明，常量的值就不能改变，但const声明的对象可以有属性变化（对象冻结Object.freeze）

		const a = [];
		a.push('Hello'); // 可执行
		a = ['Dave'];    // 报错

	也可以使用Object.freeze将对象冻结

		const foo = Object.freeze({});
		// 常规模式时，下面一行不起作用；
		// 严格模式时，该行会报错
		foo.prop = 123;//

使用let和const：

- 变量只在声明所在的块级作用域内有效

- 变量声明后方可使用（暂时性死区）

- 不能重复定义变量

- 声明的全局变量，不属于全局对象的属性

		var a = 1;
		window.a // 1
		let b = 1;
		window.b // undefined

##  this关键字 ##

我们知道，ES5函数中的this指向的是运行时所在的作用域。比如

	function foo() {
	  setTimeout(function(){
	    console.log('id:', this.id);
	  }, 100);
	}
	
	var id = 21;
	
	foo.call({id:42});//id: 21

在这里，我声明了一个函数foo,其内部为一个延迟函数setTimeout，每隔100ms打印一个this.id。我们通过`foo.call({id:42})`来调用它，并且为这个函数设定作用域。它真正执行要等到100毫秒后，由于this指向的是运行时所在的作用域，所以这里的this就指向了全局对象window，而不是函数foo。这里：

- 使用call来改变foo的执行上下文，使函数的执行上下文不再是window，从而来辨别setTimeout中的this指向

- setTimeout方法挂在window对象下，所以其this指向执行时所在的作用域——window对象。
	> 超时调用的代码都是在全局作用域中执行的，因此函数中this 的值在非严格模式下指向window 对象，在严格模式下是undefined   --《javascript高级程序设计》


为了解决这一问题，我们往常的做法往往是将this赋值给其他变量：

	function foo() {var that = this;
	  setTimeout(function(){
	    console.log('id:', that.id);
	  }, 100);
	}
		
	var id = 21;
	foo.call({id:42});//id: 42

而现在ES6推出了箭头函数解决了这一问题。

### 箭头函数 ###

标识符=> 表达式

	var sum = (num1, num2) => { return num1 + num2; }
	// 等同于
	var sum = function(num1, num2) {
	  return num1 + num2;
	};
- 如果函数只有一个参数，则可以省略`圆括号`

- 如果函数只有一条返回语句，则可以省略`大括号`和`return`

- 如果函数直接返回一个对象，必须在对象外面加上括号。(因为一个空对象{}和一个空的块 {} 看起来完全一样。所以需要用小括号包裹对象字面量。)


针对this关键字的问题，ES6规定箭头函数中的this绑定定义时所在的作用域，而不是指向运行时所在的作用域。这一以来，this指向固定化了，从而有利于封装回调函数。

	function foo() {var that = this;
	  setTimeout(()=>{
	    console.log('id:', that.id);
	  }, 100);
	}
		
	var id = 21;
	foo.call({id:42});//id: 42

**注意：**箭头函数this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this。而箭头函数根本没有自己的this，其内部的this也就是外层代码块的this。这就导致了其：

- 不能用作构造函数

- 不能用call()、apply()、bind()这些方法去改变this的指向


## 类与继承 ##

传统ECMAScript没类的概念，它描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。而实现这一行为的传统方法便是通过构造函数：
	function Point(x, y) {
	  this.x = x;
	  this.y = y;
	}
	
	Point.prototype.toString = function () {
	  return '(' + this.x + ', ' + this.y + ')';
	};
	
	var p = new Point(1, 2);

在这里，构造函数Point会有一个原型对象（prototype），这个原型对象包含一个指向Point的指针(constructor)，而实例p包含一个指向原型对象的内部指针(_prop_)。所以整个的继承是通过原型链来实现的。详情可见我的这篇文章：[javascript中的prototype和constructor](http://www.cnblogs.com/yzg1/p/5688722.html)

### class ###

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。但是类只是基于原型的面向对象模式的语法糖。对于`class`的引入，褒贬不一，很多人认为它反而是一大缺陷，但对我来说，这是一个好的语法糖，因为往常的原型链继承的方式往往能把我绕那么一会儿。

	//定义类
	class Point {
	  constructor(x, y) {
	    this.x = x;
	    this.y = y;
	  }
	
	  toString() {
	    return '(' + this.x + ', ' + this.y + ')';
	  }
	}
	var p = new Point(1, 2);

- 类里面有一个constructor方法，它是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

- constructor方法中的this关键字代表实例对象，

- 定义“类”的方法(如上例的toString)的时候，前面不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。

- 使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致

- 类的所有方法都定义在类的prototype属性上面

### class的继承——extend ###
Class之间可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。

	class ColorPoint extends Point {
	  constructor(x, y, color) {
	    super(x, y); // 调用父类的constructor(x, y)
	    this.color = color;
	  }
	
	  toString() {
	    return this.color + ' ' + super.toString(); // 调用父类的toString()
	  }
	}

- super关键字，作为函数调用时（即super(...args)），它代表父类的构造函数；作为对象调用时（即super.prop或super.method()），它代表父类。在这里，它表示父类的构造函数，用来新建父类的this对象。
- 子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。


## 模块化 ##
历史上，JavaScript一直没有模块（module）体系，无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来，这对开发大型的、复杂的项目形成了巨大障碍。为了适应大型模块的开发，社区制定了一些模块加载方案，比如CMD和AMD。

ES6的模块化写法：

	import { stat, exists, readFile } from 'fs';

上面代码的实质是从fs模块加载3个方法，其他方法不加载。这种加载称为“编译时加载”，即ES6可以在编译时就完成模块加载，效率要比CommonJS模块的加载方式高。当然，这也导致了没法引用ES6模块本身，因为它不是对象。

模块功能主要由两个命令构成：

- export

	用于规定模块的对外接口，对外的接口，必须与模块内部的变量建立一一对应关系。

		// 写法一
		export var m = 1;
		//错误
		export 1;
		
		// 写法二
		var m = 1;
		export {m};
		//错误
		export m;
		
		// 写法三  重命名
		var n = 1;
		export {n as m}; 

- import
	
	用于输入其他模块提供的功能，它接受一个对象（用大括号表示），里面指定要从其他模块导入的变量名（也可以使用*号整体加载）

	

## 字符串插值 ##

在javascript的开发中，我们常常需要这样来输出模板：

	function sayHello(name){
		return "hello,my name is "+name+" I am "+getAge(18);
	}
	function getAge(age){
		return age;
	}
	sayHello("brand") //"hello,my name is brand I am 18"

我们需要使用+来连接字符串和变量（或者表达式）。例子比较简单，所以看上去无伤大雅，但是一旦在比较复杂的情况下，就会显得相当繁琐不方便，这一用法也让我们不厌其烦。对此，ES6引入了`模板字符串`,可以方便优雅地将 JS 的值插入到字符串中。

### 模板字符串 ###

对于模板字符串，它：

- 使用反引号``包裹；
- 使用${}来输出值；
- ${}里的内容可以是任何 JavaScript 表达式，所以函数调用和算数运算等都是合法的；
- 如果一个值不是字符串，它将被转换为字符串；
- 保留所有的空格、换行和缩进，并输出到结果字符串中（可以书写多行字符串）
- 内部使用反引号和大括号需要转义，转义使用反斜杠`\`

对于上面的例子，模板字符串的写法是：

	function sayHello(name){
		return `hello,my name is ${name} I am ${getAge(18)}`;
	}
	function getAge(age){
		return age;
	}
	sayHello("brand") //"hello,my name is brandI am 18"

## 严格模式 ##
严格模式的目标之一是允许更快地调试错误。帮助开发者调试的最佳途径是当确定的问题发生时抛出相应的错误(throw errors when certain patterns occur)，而不是悄无声息地失败或者表现出奇怪的行为(非严格模式下经常发生)。严格模式下的代码会抛出更多的错误信息，能帮助开发者很快注意到一些必须立即解决的问题。在 ES5 中, 严格模式是可选项，但是在 ES6 中，许多特性要求必须使用严格模式，这个习惯有助于我们书写更好的 JavaScript。