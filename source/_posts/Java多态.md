---
title: Java多态
tags: [java面向对象]
date: 2016-04-01 16:37:47
description: java面向对象
categories: [js]
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

## 多态
> 多态是面向对象语言的又一重要特性。多态是同一个行为具有多个不同表现形式或形态的能力。多态性是对象多种表现形式的体现。Java的多态更多的是通过动态绑定体现的。


下面的程序演示了动态绑定，定义父类Animal 和子类Tiger 以及Fish
```java
/**
 * Created by yuzhigang on 2016/4/1.
 */
class Animal {
    String type;
    String name;
    int age;
    int weight;
    void breath() {
        System.out.println("动物呼吸");
    }
}
class Tiger extends Animal {
    String tigerType;
    String from;
    void breath(){
        System.out.println("老虎是用肺呼吸的");
    }
}
class Fish extends Animal{
    String fishType;
    void breath(){
        System.out.println("鱼是用腮呼吸的");
    }
}

public class DuoTai {
    public static void main(String args[ ]){
        Animal [ ]animal=new Animal[3];
       //创建不同的对象，但是都存入Animal 的引用中
        animal[0]=new Animal();
        animal[1]=new Tiger();
        animal[2]=new Fish();
        animal[0].breath();
        animal[1].breath();
        animal[2].breath();
    }
}

//动物呼吸
//老虎是用肺呼吸的
//鱼是用腮呼吸的
```
> 动态绑定是一种机制，通过这种机制，对一个已经被重写的方法的调用将会发生在运行时，而不是在编译时解析。

在 Java 中，对象是多态的，定义一个Animal 对象，它既可以存放Animal 对象，也可以存放Animal 的子类Tiger和Fish 的对象。存放在 Animal 数组中的Tiger 对象和Fish 对象在执行breath 方法时会自动地调用原来对象的方法而不是Animal 的breath 方法，这就是动态绑定。

**注意：通过数组元素访问方法的时候只能访问在Animal 中定义的方法，对于Tiger 类和Fish 中定义的方法时却不能调用，比如这里的breath方法在animal中是定义过的**











我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！