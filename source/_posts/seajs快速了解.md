---
title: seajs快速了解
date: 2015-12-17 15:04:34
tags: [web]
description: SeaJS
categories: [js]
---

> SeaJS是一个遵循CommonJS规范的JavaScript模块加载框架，可以实现JavaScript的模块化开发及加载机制。与jQuery等JavaScript框架不同，SeaJS不会扩展封装语言特性，而只是实现JavaScript的模块化及按模块加载。SeaJS的主要目的是令JavaScript开发模块化并可以轻松愉悦进行加载，将前端工程师从繁重的JavaScript文件及对象依赖处理中解放出来，可以专注于代码本身的逻辑。SeaJS可以与jQuery这类框架完美集成。使用SeaJS可以提高JavaScript代码的可读性和清晰度，解决目前JavaScript编程中普遍存在的依赖关系混乱和代码纠缠等问题，方便代码的编写和维护。
SeaJS的作者是前淘宝UED,现支付宝前端工程师玉伯。

<!-- more -->

## 特点
SeaJS 追求简单、自然的代码书写和组织方式，具有以下核心特性：

+ 简单友好的模块定义规范：SeaJS 遵循CMD规范，可以像Node.js一般书写模块代码。
+ 自然直观的代码组织方式：依赖的自动加载、配置的简洁清晰，可以让我们更多地享受编码的乐趣。


## 使用
SeaJS是为了实现JavaScript的模块化的，所以在 SeaJS 中，所有 JavaScript 文件都应该用模块的形式来书写，并且一个文件只包含一个模块。它的操作也是围绕着模块来进行。

1.定义模块——**defined**

	define(id, dependencies,function(require, exports, module) {

	  // 模块代码

	});

+ id:当前模块的唯一标识。该参数可选。如果没有指定，默认为模块所在文件的访问路径。如果指定的话， 必须是顶级或绝对标识（不能是相对标识）。
+ dependencies:当前模块所依赖的模块，是一个由模块标识组成的数组。该参数可选。
+ fn:模块的工厂函数。模块初始化时，会调用且仅调用一次该工厂函数。
	+ require:用来获取指定模块的接口
``` javascript
	define(function(require) {

	  // 获取模块 a 的接口
	  var a = require('./a');

	  // 调用模块 a 的方法
	  a.doSomething();
	});
```
	+ require.async:用来在模块内部异步加载一个或多个模块。

	``` javascript
		define(function(require) {

		  // 异步加载多个模块，在加载完成时，执行回调
		  require.async(['./c', './d'], function(c, d) {
		    c.doSomething();
		    d.doSomething();
		  });

		});
	```

2.配置——**seajs.config**

```javascript
seajs.config({

  // 设置路径，方便跨目录调用
  paths: {
    'arale': 'https://a.alipayobjects.com/arale',
    'jquery': 'https://a.alipayobjects.com/jquery'
  },

  // 设置别名，方便调用
  alias: {
    'class': 'arale/class/1.0.0/class',
    'jquery': 'jquery/jquery/1.10.1/jquery'
  }

});
```

3.加载模块——**seajs.use**

``` javascript
/ 加载一个模块
seajs.use('./a');

// 加载一个模块，在加载完成时，执行回调
seajs.use('./a', function(a) {
  a.doSomething();
});

// 加载多个模块，在加载完成时，执行回调
seajs.use(['./a', './b'], function(a, b) {
  a.doSomething();
  b.doSomething();
});
```