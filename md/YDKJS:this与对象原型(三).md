# YDKJS:this 与对象原型(三)

<!-- TOC -->

- [YDKJS:this 与对象原型(三)](#ydkjsthis-与对象原型三)
    - [类和面向对象](#类和面向对象)
        - [类的设计模式](#类的设计模式)
        - [类理论](#类理论)
        - [类机制](#类机制)
            - [建造](#建造)
            - [构造函数](#构造函数)
        - [类继承](#类继承)
            - [多态](#多态)
            - [多重继承](#多重继承)
    - [javascript 中的类](#javascript-中的类)
        - [混入](#混入)
            - [显式混入](#显式混入)
            - [隐式混入](#隐式混入)
        - [寄生](#寄生)

<!-- /TOC -->

## 类和面向对象

类是一种设计模式：实例化(instantiation)、继承(inheritance)和（相对）多态(polymorphism)。

### 类的设计模式

**类是非常常用的一种设计模式**。

### 类理论

类／继承描述了代码的一种组织结构形式。

面向对象编程强调的是数据和操作数据的行为本质上是互相关联的，因此好的设计就是把数据以及和它相关的行为打包(或者说封装)起来。

举例来说，一个单词是一个字符串，它就是数据，但它往往还需要很多行为（截断、拼接等）。 如果采用面向对象的方式，将这种数据和行为封装在一起，就设计成了`String`类。

### 类机制

#### 建造

“类”和“实例”的概念来源于房屋建造。一个类就是一张蓝图，按照这个蓝图所建造的具体建筑物就是实例。

> 类意味着**复制**。

#### 构造函数

构造函数的特点：

* 是一个类方法
* 通常与类同名
* 大多需要 `new` 来调用

### 类继承

可以先定义一个类，然后定义一个继承前者的类，后者通常被称为“子类”，前者通常被称为“父类”。

“子类”和“父类”二者之间没有直接的联系。定义好一个子类之后，相对于父类来说它就是一个独立并且完全不同的类。子类会包含父类行为的原始副本，但是也可以重写所有继承的行为甚至定义新行为。

```
// 语法自编
class Vehicle {
    engines = 1
    ignition() {
        output( "Turning on my engine." );
    }
    drive() {
        ignition();
        output( "Steering and moving forward!" )
    }
}
class Car inherits Vehicle { wheels = 4
    drive() {
        inherited:drive()
        output( "Rolling on all ", wheels, " wheels!" )
    }
}
class SpeedBoat inherits Vehicle { engines = 2
    ignition() {
        output( "Turning on my ", engines, " engines." )
    }
    pilot() {
        inherited:drive()
        output( "Speeding through the water with ease!" )
    }
}
```

#### 多态

> 子类的方法中引用继承来的原始方法，这个技术被称为多态或者虚拟多态。

多态是一个非常广泛的话题，它包含多个方面：

* 任何方法都可以引用继承层次中高层的方法(无论高层的方法名和当前方法名是否相同)。
* 在继承链的不同层次中一个方法名可以被多次定义，当调用方法时 会自动选择合适的定义。

#### 多重继承

有些语言允许你继承多个“父类”，多重继承意味着所有父类的定义都会被复制到子类中。

JavaScript 要本身并不提供“多重继承”功能。但开发者们尝试了各种各样的办法来实现多重继承。

## javascript 中的类

> JavaScript 中实际上并没有有类

但由于类是一种设计模式，所以我们可以用一些方法近似实现类的功能。 为了满足对于类设计模式的最普遍需求，JavaScript 提供了一些近似类的语法。

### 混入

混入：javascript 中类的实现。

> 在继承或者实例化时，JavaScript 的对象机制并不会自动执行复制行为。

JavaScript 中只有对象，并不存在可以被实例化的“类”。一个对象并不会被复制到其他对象，它们会被关联起来。

#### 显式混入

JavaScript 不会自动实现类的复制行为，所以我们需要手动实现复制功能。

```js
function mixin(sourceObj, targetObj) {
    for (var key in sourceObj) {
        // 只会在不存在的情况下复制
        if (!(key in targetObj)) {
            targetObj[key] = sourceObj[key];
        }
    }
    return targetObj;
}
var Vehicle = {
    engines: 1,
    ignition: function() {
        console.log("点火");
    },
    drive: function() {
        this.ignition();
        console.log("行驶");
    }
};
var Car = mixin(Vehicle, {
    wheels: 4,
    drive: function() {
        Vehicle.drive.call(this);
        console.log("Rolling on all " + this.wheels + " wheels!");
    }
});
```

这里我们处理的已经不再是类了，因为在 JavaScript 中不存在 类，Vehicle 和 Car 都是对象，供我们分别进行复制和粘贴。

复制操作完成后，Car 就和 Vehicle 分离了，向 Car 中添加属性不会影响 Vehicle，反之 亦然。

缺点：

1. 显式混入实际上无法完全模拟类的复制行为，，因为对象(函数也是对象)只能复制引用，无法复制被引用的对象或者函数本身。
2. 并不能带来太多的好处，无非就是少几条定义语句，

#### 隐式混入

```js
var Something = {
    cool: function() {
        this.greeting = "Hello World";
        this.count = this.count ? this.count + 1 : 1;
    }
};

Something.cool();
Something.greeting; // "Hello World"
Something.count; // 1
var Another = {
    cool: function() {
        // 隐式把 Something 混入 Another
        Something.cool.call(this);
    }
};
Another.cool();
Another.greeting; // "Hello World"
Another.count; // 1(count 不是共享状态)
```

但是`Something.cool.call( this )`仍然无法变成相对(而且更灵活的)引用，所以使用时千万要小心。通常来说，尽量避免使用这样的结构，以保证代码的整洁和可维护性。

### 寄生

显式混入模式的一种变体被称为“寄生继承”，它既是显式的又是隐式的

```js
//“传统的 JavaScript 类”Vehicle
function Vehicle() {
    this.engines = 1;
}

Vehicle.prototype.ignition = function() {
    console.log("点火");
};
Vehicle.prototype.drive = function() {
    this.ignition();
    console.log("前进");
};

//“寄生类”Car
function Car() {
    // 首先，car 是一个 Vehicle
    var car = new Vehicle();
    // 接着我们对 car 进行定制
    car.wheels = 4;
    // 保存到 Vehicle::drive() 的特殊引用
    var vehDrive = car.drive;
    // 重写 Vehicle::drive()

    car.drive = function() {
        vehDrive.call(this);
        console.log(this.wheels + "个轮子在跑");
    };
    return car;
}
var myCar = new Car();
myCar.drive();
```
