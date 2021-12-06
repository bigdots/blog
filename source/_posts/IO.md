---
title: IO
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

I/O： 输入/输出(Input/Output)。

## Buffer

基于 `TypedArray`( ES6 新增) 中 `Uint8Array` 来实现。

特点：

+ 大小固定不变
+ 在 V8 堆栈外分配原始内存空间


使用方法：

+ Buffer.from()	 根据已有数据生成一个 Buffer 对象
+ Buffer.alloc()	创建一个初始化后的 Buffer 对象
+ Buffer.allocUnsafe()	创建一个未初始化的 Buffer 对象


## 流( Stream )

流 (stream) 是一种很早之前流行的编程方式（C语言）。

应用的场景：拷贝一个 20G 大的文件, 如果你一次性将 20G 的数据读入到内存, 你的内存条可能不够用, 或者严重影响性能。
但是你如果使用一个 1MB 大小的缓存 (buf) 每次读取 1Mb, 然后写入 1Mb, 那么不论这个文件多大都只会占用 1Mb 的内存.

Stream 的类型