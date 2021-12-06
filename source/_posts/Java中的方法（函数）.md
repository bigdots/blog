---
title: Java中的方法（函数）
tags: [java基础]
date: 2016-03-31 13:48:25
description: Java中的方法（函数）
---
## 定义
> 访问修饰符 返回值类型 方法名(参数列表){
    //code...
}

<!-- more -->

+ 修饰符：方法允许被访问的权限范围，可以是 public、protected、private 甚至可以省略 ，其中 public 表示该方法可以被其他任何代码调用
+ 返回值类型 ：方法可能会返回值。returnValueType是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType是关键字`void`。
+ 方法名：定义的方法的名字
+ 参数列表：传递给方法的参数列表，参数可以有多个，多个参数间以逗号隔开，每个参数由参数类型和参数名组成，以空格隔开

## 调用
+ 方法有返回值，方法调用通常被当做一个值；
+ 方法返回void，方法调用通常直接执行；

## 方法重载
在 Java 中支持有两个或多个同名的方法，但是它们的参数个数和类型必须有差别。这种情况就是方法重载（overloading）。重载是Java 实现多态的方式之一。当调用这些同名的方法时，Java 根据参数类型和参数的数目来确定到底调用哪一个方法，注意返回值类型并不起到区别方法的作用。
```java
public class OverLoadDemo {
        //定义一系列的方法，这些方法的参数是不同的，通过参数来区别调用的方法
        void method(){
            System.out.println("无参数方法被调用");
        }
        void method(int a){
            System.out.println("参数为int 类型被调用");
        }
        void method(double d){
            System.out.println("参数为double 方法被调用");
        }
        void method(String s){
            System.out.println("参数为String 方法被调用");
        }
        public static void main(String args[ ]){
            OverLoadDemo ov=new OverLoadDemo();
            //使用不同的参数调用方法
            ov.method();
            ov.method(4);
            ov.method(4.5D);
            ov.method("a String");
        }
}
```

## 变量作用域
+ 变量的范围是程序中该变量可以被引用的部分。
+ 方法内定义的变量被称为局部变量。
+ 局部变量的作用范围从声明开始，直到包含它的块结束。
+ 局部变量必须声明才可以使用。
+ 方法的参数范围涵盖整个方法。参数实际上是一个局部变量。
+ for循环的初始化部分声明的变量，其作用范围在整个循环。但循环体内声明的变量其适用范围是从它声明到循环体结束。






















我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！