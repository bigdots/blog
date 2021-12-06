# YDKJS:this 与对象原型(四)


## 原型


### [[Prototype]]

[[Prototype]] 是 JavaScript 对象中的内置属性。几乎所有的对象在创建时 [[Prototype]] 属性都会被赋予一个非空的值。

1. 作用：
保持对另一个对象的引用。
[[Get]] 操作当被检查对象本身不具有有某个属性时就会使用对象的 [[Prototype]] 链查找。

2. 尽头
[[Prototype]] 链的“尽头” 是Object.prototype 对象。


### 类

在 JavaScript 中，我们并不会将一个对象(“类”)复制到另一个对象(“实例”)，只是将它们关联起来，称为原型继承。