---
title: Java的类和对象
tags: [java面向对象]
date: 2016-03-31 09:25:25
description: Java的类和对象
categories: [js]
---

## 类
类实际上是定义一个模板，而对象是由这个模板产生的一个实例。

<!-- more -->

**类的一般形式**
>class 类名{
 类型实例变量名;
 类型实例变量名;
 ……
 类型 方法名(参数){
 //方法内容
 }
 ……
}

这里，在类名面前并没有向以前那样加上修饰符public，在Java 中是允许把许多类的声明放在一个Java 文件中的，但是这些类**只能有一个类被声明为public**，而且这个**类名必须和Java 文件名相同**。
类修饰符：
 + private：只有本类可见。
 + protected：本类、子类、同一包的类可见。
 + 默认（无修饰符）：本类、同一包的类可见。
 + public：对任何类可见。

```java
class Humans {
    String name;
    String sex;
    int age;
    String address;
    void work(){
        System.out.println("I am working");
    };
    void eat(){
        System.out.println("I am eating");
    };
    String getState(int time){
        String state = null;
        if(time>=0&& time<=24){
            if(time>8&&time<17){
                state="I am working";
            }else if(time>=17&&time<22){
                state="I am studying";
            }else{
                state="I am sleeping";
            }
        }else{
            state = "it's not correct time";
        }
        return state;
    }
}
//测试类
public class Human {
    public static void main(String args[ ]) {
        //创建对象
        Humans wangming = new Humans();
        //对象的使用
        wangming.name = "王明";
        wangming.age = 25;
        wangming.sex = "男";
        wangming.address = "中国北京";
        System.out.println(wangming.name+"晚上23 点钟你在干嘛");
        //调用getState()方法，把返回值打印出来
        System.out.println(wangming.getState(23));
        System.out.println("下午15 点呢");
        System.out.println(wangming.getState(15));
    }
}
```

## 对象
描述对象的类抽象出来，已经有了建立对象的模板，接下来的工作就是创造这些类的实例，既对象
**创建对象**
> className objectName = new className();

1. 上面的创建语句其实是俩个语句合并而成：
 + 左边`className objectName`语句声明一个className类的变量objectName，
 + 右边`new className()`语句通过new 运算符获得一个对象实例并为其分配内存，获得对象实例赋值给wangming
2. 这句语句实际上是调用了一个方法，这个方法是系统自带的方法，由于这个方法被用来构造对象，所以把它称为构造函数。构造函数的作用是生成对象，并对对象的实例变量进行初始化。系统自带的默认构造函数把所有的数字变量设为0，把所有的boolean 型变量设为false，把所有的对象变量都设为null。
把例1中的Humans类实例化，即Humans wangming = new Humans();结果如下：
```java
public Humans(){
    name=null;
    age=0;
    sex=null;
    addr=null;
}
```
最后把这个赋值给wangming变量。

构造函数有一个很明显的特点是它的名字必须跟类名相同，并且没有返回值类型


**使用对象**
对象是使用主要通过设置对象属性和调用对象方法的方式，见例1。


## 包
在大型的项目中，可能需要上千个类甚至上万个类，如果都放在一起，是非常乱的，而且要对这上万个类都起不同的名字，显然这样是很复杂的。Java 提供了一种有效的类的组织结构，这就是包。包机制是为了更好地组织类，用于区别类名的命名空间。

**包的作用**
+ 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用。
+ 如同文件夹一样，包也采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。因此，包可以避免名字冲突。
+ 包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类。



**创建**
 + 创建一个新文件夹，这个文件夹就是包；
 + 在这个文件夹下创建各个程序，但在每个程序前加上`package pkg;`声明：
    ```
    package pkg;
    public class PkgTest{
        public static void main(String args[ ]){
            System.out.println("PkgTest success");
        }
    }
    ```

**导入包**
包可以对类进行良好的管理，但是这样的话包就位于不同的文件夹下面了，不能直接在一个文件夹中调用需要的类。Java 的解决方案是导入需要的包，调用包主要通过`import`来实现。
`import packageName.*`导入包中所有的类



我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)
如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！