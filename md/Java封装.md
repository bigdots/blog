title: Java封装
tags: [java面向对象]
date: 2016-04-05 15:52:54
description: Java封装
---

封装（Encapsulation）一种将抽象性函式接口的实作细节部份包装、隐藏起来的方法。
封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。要访问该类的代码和数据，必须通过严格的接口控制。
封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段。适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性。

<!-- more -->

### 实现
```java
public class EncapTest{
    private int age;
    public int getAge(){
        return age;
    }
    public void setAge( int newAge){
        age = newAge;
    }
}
```
上面的例子中，定义了一个私有变量和一个public函数。public方法是外部类访问该类成员变量的入口。通常情况下，这些方法被称为getter和setter方法。因此，任何要访问类中私有成员变量的类都要通过这些getter和setter方法。

通过如下的例子说明EncapTest类的变量怎样被访问：

```java
public static void main(String args[]){
        EncapTest encap = new EncapTest();
        encap.setAge(20);
        System.out.print("age : " + encap.getAge());
}
```

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)

如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！
