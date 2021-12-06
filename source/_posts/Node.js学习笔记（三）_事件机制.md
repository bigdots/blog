---
title: Node.js学习笔记（三）_事件机制
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---


大部分的nodejs核心api都建立在异步的事件驱动架构之上，所以events是Node.js 最重要的模块,它提供了唯一的接口。events 模块不仅用于用户代码与 Node.js 下层事件循环的交互,还几乎被所有的模块依赖。

## EventEmitter

events 模块只提供了一个对象: events.EventEmitter。EventEmitter的核心就是事件发射与事件听器功能的封装。所有发射事件的对象都是EventEmitter类的实例，它暴露一个on函数来绑定一个或着多个函数到该对象上。当事件发射时,注册到这个事件的事件监听器被依次调用,事件参数作为回调函数参数传递。


```
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('an event occurred!');
});
myEmitter.emit('event');
```

以上例 中, myEmitter 为事件 event 注册了一个事件监听器, 后发射了 event 事件。运行结果中可以看到这个事件监听器的回调函数被调用。


+ EventEmitter.on(event, listener) 为指定事件注册一个监听器,接受一个字
符串 event 和一个回调函数listener。
+ EventEmitter.emit(event, [arg1], [arg2], [...]) 发射 event 事件,传
递若干可选参数到事件监听器的参数表。
+ EventEmitter.once(event, listener) 为指定事件注册一个单次监听器, 监听
听器最多只会触发一次,触发后立刻解除该监听器。
+ EventEmitter.removeListener(event, listener)  移除指定事件的某个监听
器,listener必须是该事件已经注册过的监听器。



如果对一个事件添加了超过10个侦听器,将会得到一条警告,这 一处设计与Node.js自身单线程运行有关,设计者认为侦听器太多,可能导致内 存泄漏,所以存在这样一个警告.


## error事件
EventEmitter 定义了一个特殊的事件 error,它包含了“错误”的语义,我们在遇到异常的时候通常会发射 error 事件。当error 被发射时,EventEmitter 规定如果没有响应的监听器,Node.js 会把它当作异常, 退出程序并打印调用栈。我们一般要为会发射 error 事件的对象设置监听器,避免遇到错误后整个个程序崩溃。

最佳实践：

```
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
  console.log('whoops! there was an error');
});
myEmitter.emit('error', new Error('whoops!'));
```




## http网络操作

## 进程管理