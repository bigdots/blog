title: Java抽象类
tags: [java面向对象]
date: 2016-04-05 14:56:32
description: Java抽象类
---


在面向对象的概念中，所有的对象都是通过类来描绘的，但并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。
抽象类是指在类中定义方法，但是并不去实现它，而在它的子类中去具体的实现。定义的抽象方法不过是一个方法占位符。继承抽象类的子类必须实现父类的抽象方法，除非子类也被定义成一个抽象类。

<!-- more -->

**关键词：**
+ 不能实例化对象，但其它功能和普通类一样
+ 在类中定义方法，但是并不去实现它
+ 子类必须实现父类的抽象方法


### 声明
1. 抽象类声明
> 修饰符 abstract 类名{
//类体
}

2. 抽象方法声明
> 修饰符 abstract 返回值类型方法名();

抽象方法没有定义，方法名后面直接跟一个分号，而不是花括号。

声明抽象方法会造成以下两个结果：
+ 如果一个类包含抽象方法，那么该类必须是抽象类。
+ 任何子类必须重写父类的抽象方法，或者声明自身为抽象类。

**在抽象类中的方法不一定是抽象方法，但是含有抽象方法的类必须被定义成抽象类**

```
//抽象类声明
abstract class Animal {
    String type;
    String name;
    int age;
    int weight;
    void eat() {
        System.out.println("动物爱吃饭");
    }
    //抽象方法声明
    abstract void breath();
}

```
### 使用
```
 public static void  main(String args[]){
    //error
    //Animal animal1 = new Animal();
    //抽象类可以创建类对象变量，只是这个变量只能用来指向它的非抽象子类对象
    Animal animal1 = new Tiger();
    animal1.breath();
    //不能直接调用tiger子类的方法
    //animal1.run();
    //正确的调用方法
    ((Tiger)animal1).run();
}
```

+ 由于根本不可能构建出Animal 对象，所以animal1存放的对象是Tiger对象，它会动态绑定正确的方法进行调用。
+ 尽管animal1存放的是Tiger 对象，但是不能直接调用tiger子类的方法，






















我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！
