---
title: Node.js学习笔记（二）_模块
date: 2021-12-06 19:57:35
tags:
description:
categories: [js]
---

模块是 Node.js 应用程序的基本组成部分,文件和模块是一一对应的。一个 Node.js 文件就是一个模块,这个文件可能是 JavaScript 代码、JSON 或者编译过的 C/C++ 扩展。

由于JavaScript没有模块系统，所以Node.js依靠CommonJS规范自身实现了模块系统。

## 模块的简单使用——exports 、require 和 module

在编写和使用每个模块时，Node.js都有require、exports、module三个预先定义好的变量可供使用。

1. exports

	`exports`对象是当前模块的导出对象，用于导出模块公有方法和属性。
	事实上,exports 本身仅仅是一个普通的空对象,即 {},它专门用来声明接口。例如：
	
	```
	//module.js
	exports.sayHello = function(name) { 
		console.log('Hello ' 	+ name);
	};
	```	
2.  require

	`require`函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以./开头），或者是绝对路径（以/或C:之类的盘符开头）。另外，模块名中的.js扩展名可以省略。例如：
	
	```
	//index.js
	var myModule = require('./module');
	myModule.sayHello("node");
	```	
3. module

	通过module对象可以访问到当前模块的一些相关信息，但最多的用途是覆盖 exports。例如模块导出对象默认是一个普通对象，如果想改成一个函数的话：
	
	```
	//module.js
	module.exports = function(name) { 
		console.log('Hello ' 	+ name);
	};
	
	//index.js
	var sayHello = require('./module');
	sayHello("node");
	```	
	
## 模块进阶——模块载入策略

Node.js的模块分为两类，一类为原生（核心）模块，一类为文件模块。原生模块在Node.js源代码编译的时候编译进了二进制执行文件，加载的速度最快。另一类文件模块是动态加载的，加载速度比原生模块慢。

### 内部实现机制

加载文件模块的工作，主要由原生模块module来实现和完成，该原生模块在启动时已经被加载，进程直接调用到runMain静态方法。

**Module源码：**
	
```
function Module(id, parent) {
  this.id = id;
  this.exports = {};
  this.parent = parent;
  if (parent && parent.children) {
    parent.children.push(this);
  }
	
  this.filename = null;
  this.loaded = false;
  this.children = [];
}
```

模块解析流程：

1. 命令行执行主模块
	
	命令行执行主模块

	```
	// bootstrap main module.
	Module.runMain = function() {
	  // Load the main module--the command line argument.
	  Module._load(process.argv[1], null, true);
	  // Handle any nextTicks added in the first tick of the program
	  process._tickCallback();
	};
	```
	
	

2. 处理模块

	Module.runMain方法会在最后执行_load静态方法，该方法又会在分析文件名之后执行:

	```
	//实例化Module函数
	var module = new Module(id, parent);
	```
	
	并根据文件路径**缓存**当前模块对象，该模块实例对象则根据文件名加载。
	
	```
	module.load(filename);
	```
	
	这时，Node.js会根据不同文件模块类型的后缀名来决定加载方法。

	+ js：通过fs模块同步读取js文件并编译执行。
	+ node：通过C/C++进行编写的Addon。通过dlopen方法进行加载。
	+ json：读取文件，调用JSON.parse解析加载。
	
	
	
3. 输出结果

	最后js文件形式的模块会变成以下形式的内容：
	
	```
	(function (exports, require, module, __filename, __dirname) {
    	var circle = require('./circle.js');
	    	console.log('The area of a circle of radius 4 is ' +circle.area(4));
	});
	```
	所以此时，主模块内可以使用exports, require, module等变量了，而其他模块又会通过require引进主模块，require方法会同runMain一样调用_load静态方法，以此类推。
	
	**require源码:**
	
	```
	// 传入模块路径作为参数. 返回 模块的exports属性.
	
	Module.prototype.require = function(path) {
	  assert(path, 'missing path');
	  assert(typeof path === 'string', 'path must be a string');
	  return Module._load(path, this, /* isMain */ false);
	};
	```


## Q&A
1. console.log(this)在浏览器和Node中分别打印出什么？
	
	答：显然浏览器中直接打印this指向Window对象，而在Node中，我们编写的文件其实外面都包裹了一层函数，而且该函数执行时强制apply将this指向了module.exports，因此此处打印为{}。

2. 为什么require、__filename、__dirname、module、exports等几个变量并没有定义在app.js 文件中，但是这个方法却存在的原因。

	答：这里提到的所有属性均是_load方法中我们编写的js文件外层包裹函数提供给我们的，因此可以直接调用。