---
title: Node.js学习笔记（四）_全局对象
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

在浏览器 JavaScript 中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性。

这里列出的所有变量均可以在所有模块中访问。但值得注意的是：它们有些并不是在全局作用域内的，即不在global对象内,而是在模块作用域内的。在模块作用域内的变量会用*表示。

### global

全局对象，全局变量的宿主

### *__filename

表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。

### *__dirname

表示当前执行脚本所在的目录。

### *exports

`module.exports`的引用，用于导出模块对象。

### *module

当前模块的引用，提供`module.exports`方法导出模块，并且允许通过require导入模块。

### *require()

用于导入模块对象。

### *require.cache

缓存模块的对象，当模块被require后缓存在此处。可以通过删除这个模块内的`key`值，删除缓存。

### *require.resolve()

使用node内部的require机制查看本地模块，它会返回模块的文件名但不会引入该模块。

### clearImmediate(immediateObject)

全局函数用于清除一个之前通过 setImmediate() 创建的Immediate对象。 参数 immediateObject 是通过 setImmediate() 函数创建的Immediate对象。

### clearInterval(intervalObject)

全局函数用于清除一个之前通过 setInterval() 创建的定时器。 参数 intervalObject 是通过 setInterval() 函数创建的定时器。

### clearTimeout(timeoutObject)

全局函数用于清除一个之前通过 setTimeout() 创建的定时器。 参数 timeoutObject 是通过 setTimeout() 函数创建的定时器。

### console

用于提供控制台标准输出。

### process

用于描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口。

### setImmediate(callback[, ...args])

立即执行指定函数，它在异步I/O之后、settimeout()和setinterval()定时器之前调用。返回一个供clearImmediate使用的Immediate对象

### setInterval(callback, delay[, ...args])

在指定的毫秒(ms)数后执行指定函数(cb)。返回一个代表定时器的句柄值。setInterval() 方法会不停地调用函数。

### setTimeout(callback, delay[, ...args])

在指定的毫秒(ms)数后执行指定函数(callback)。：setTimeout() 只执行一次指定函数。返回一个代表定时器的句柄值。

### Buffer

用于处理二进制数据，详见buffer部分
