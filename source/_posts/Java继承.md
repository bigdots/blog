---
title: Java继承
tags: [java面向对象]
date: 2016-03-31 14:19:14
description: Java面向对象-继承
---
## 万物皆对象

Java 是一门面向对象的语言，其重要的一个思想就是“万物皆对象”。而类是Java 的核心内容，它是一种逻辑结构，定义了对象的结构，可以由一个类得到众多相似的对象。从某种意义上说，类是Java 面向对象性的基础。

<!-- more -->

Java作为一种面向对象语言。支持以下基本概念：
 + 继承
 + 多态
 + 封装
 + 抽象
 + 类
 + 对象
 + 实例
 + 方法
 + 重载

## 继承
> 继承是java面向对象编程技术的一块基石，因为它允许创建分等级层次的类。继承可以理解为一个对象从另一个对象获取属性的过程。在Java中，类的继承是单一继承，也就是说，一个子类(派生类)只能拥有一个父类

### 继承的实现
继承的实现主要通过`extends`和`implements`来实现

```
class Animal{
}
//tiger子类继承animal父类
class tiger extends Animal{

}
```
### 子类对象的构建
从最顶层的基类开始往下一层层的调用默认构造函数
```java
class Animals{
    Animals(){  //无参构造器
        System.out.println("animal");
    }
}

class Mammal extends Animals{
    Mammal(){
        System.out.println("Mammal");
    }
}

class Dog extends Mammal{
    Dog(){
        System.out.println("Dog");
    }
}
public class ClassTransmit {
    public static void main(String args[]){
        Dog alone = new Dog();
    }
}


//animal
//Mammal
//Dog

```
在上面的程序中定义了3 个类Animals、Mammal、Dog，其中Mammal继承自Animals，Dog 继承自Mammal，当创建一个Dog 类的对象时候，会自动调用父类的无参构造函数，从上面的函数运行结果可以看出，构造函数的调用顺序为animal、Mammal、Dog。是自顶向下的。

**instanceof 关键字**
可以使用 instanceof 运算符来检验一个对象是否是一个类的一个实例。

### 方法的覆写
通过在子类中重写父类的方法，从而达到适应子类的特殊情况的效果。
```
class Tiger extends Animal {
    void breath(){ //呼吸方法
        System.out.println("tiger breath");
    }
}

public class OverLoad {
    public static void main(String args[]){
        Tiger woods = new Tiger();
        woods.breath(); //tiger breath
    }
}
```
上面程序中，animal父类有一个breath方法，输出的是“animal breath”；这里在子类中改写了它的方法，使它输出“tiger breath”

## final
编写程序时可能需要把类定义为不能继承的，即最终类，或者是有的方法不希望被子类继承，这时候就需要使用final 关键字来声明:
> final class 类名 extends 父类{
//类体
}

方法也可以被声明为final:
>修饰符 final 返回值类型方法名(){
//方法体
}

**实例变量也可以被定义为final，被定义为final 的变量不能被修改。被声明为final 的类的方法自动地被声明为final，但是它的实例变量并不是final。**

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！