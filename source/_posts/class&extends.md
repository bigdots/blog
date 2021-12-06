---
title: class&extends
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

## class

```js
class Welcome {
    render() {
        return 1;
    }
}
```

原理：

1. 声明一个构造函数
1. this 上的属性通过构造函数的 this 挂载
1. 其他属性通过 Object.defineProperty 挂载到`构造函数/构造函数的原型`上

```js
"use strict";

var _createClass = (function() {
    function defineProperties(target, props) {
        for (var i = 0; i < props.length; i++) {
            var descriptor = props[i];
            descriptor.enumerable = descriptor.enumerable || false;
            descriptor.configurable = true; // 可配置
            if ("value" in descriptor) descriptor.writable = true; // 存在value属性则可编辑
            Object.defineProperty(target, descriptor.key, descriptor);
        }
    }
    return function(Constructor, protoProps, staticProps) {
        if (protoProps) defineProperties(Constructor.prototype, protoProps);
        if (staticProps) defineProperties(Constructor, staticProps);
        return Constructor;
    };
})();

var Welcome = (function() {
    function Welcome() {
        this.a = 1;
    }

    _createClass(Welcome, [
        {
            key: "render",
            value: function render() {
                return 1;
            }
        }
    ]);

    return Welcome;
})();
```

## extends

主要利用了 Object.create。

```js
class C extends Welcome {}
```

```js
function _inherits(subClass, superClass) {
    if (typeof superClass !== "function" && superClass !== null) {
        throw new TypeError(
            "Super expression must either be null or a function, not " +
                typeof superClass
        );
    }
    subClass.prototype = Object.create(superClass && superClass.prototype, {
        constructor: {
            value: subClass,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    if (superClass)
        Object.setPrototypeOf
            ? Object.setPrototypeOf(subClass, superClass)
            : (subClass.__proto__ = superClass);
}

```