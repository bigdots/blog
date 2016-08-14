title: Java异常处理
tags: [java基础]
date: 2016-04-07 13:37:15
description: Java异常处理
---

由于硬件问题、资源耗尽、输入错误以及程序代码编写不严谨会导致程序运行时产生异常，这些异常会导致程序中断而不能正常继续运行，所以需要对异常的情况进行妥善处理。Java 提供了一套完善的异常处理机制，通过这套机制，可以轻松地写出容错性非常高的代码。

<!-- more -->

## 异常
Java 的异常实际上是一个对象，这个对象描述了代码中出现的异常情况。在代码运行异常时，在有异常的方法中创建并抛出一个表示异常的对象，然后在相应的异常处理模块中进行处理。

在Java中，所有的异常类是从java.lang.Exception类继承的子类。而Exception类又是Throwable类的子类。除了Exception类外，Throwable还有一个子类Error。如图所示：

![](/images/201601/ErrorClass.png)

+ 它的一个分支是 Error，它定义了Java 运行时的内部错误。通常用Error 类来指明与运行环境相关的错误。应用程序不应该抛出这类异常，发生这类异常时通常是无法处理的，它们在Java程序处理的范畴之外。例如，JVM内存溢出。一般地，程序不会从错误中恢复。

+ 另一分支是 Exception，它用于程序中应该捕获的异常。Exception 类也有两个分支，一个是RuntimeException，另一个分支是IOException。RuntimeException 主要用来描述由于编程产生的错误

**Java有以下三种类型的异常：**
+ 检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
+ 运行时异常： 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
+ 错误： 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

## Java 内置异常类
Java 语言定义了一些异常类在java.lang标准包中。标准运行时异常类的子类是最常见的异常类。由于java.lang包是默认加载到所有的Java程序的，所以大部分从运行时异常类继承而来的异常都可以直接使用。

**非检查性异常**
![](/images/201601/javaError.png)

**检查性异常类**
![](/images/201601/javaNotError.png)

## 异常方法
![](/images/201601/javaErrorMethod.png)


**1. 异常捕获**
使用try和catch关键字可以捕获异常。try-catch代码块放在异常可能发生的地方

> try{
//可能出现异常代码
}
catch(异常类型1 异常对象)
{
//对异常1 的处理
}
catch(异常类型2 异常对象)
{
//对异常2 的处理
}
┆
catch(异常对象n 异常对象)
{
//异常对象n 的处理
}

实例：
```java
public class ExcepTest {
    public static void main(String args[]){
        try{
            int a[] = new int[2];
            System.out.println("Access element three :" + a[3]);
        }catch(ArrayIndexOutOfBoundsException e){
            System.out.println("Exception thrown  :" + e);
        }
        System.out.println("Out of the block");
    }
}
//result
//Exception thrown  :java.lang.ArrayIndexOutOfBoundsException: 3
// Out of the block
```
**2.获取异常的堆栈信息**
在异常类中有一个非常重要的方法printStackTrace()，该方法将在Throwable 类中定义，它的作用是把Throwable 对象的堆栈信息输出至输出流。
示例：
```
public class TestprintStackTraceDemo {
    public static void main(String args[ ]){
        try //对method1 方法进行异常处理
        {
            method1(); //调用method1 方法
        }
        catch(NullPointerException e)
        {
            e.printStackTrace(); //获取异常信息
        }
    }
    static void method1()
    {
        method2(); //调用method2 方法
    }
    static void method2()
    {
        method3(); //调用method3 方法
    }
    static void method3()
    {
        String str=null; //字符串的值为null
        int n=str.length(); //获取字符串的长度
    }
}
```
运行结果如下：它打印出了异常的栈信息
```
java.lang.NullPointerException
    at TestprintStackTraceDemo.method3(TestprintStackTraceDemo.java:26)
    at TestprintStackTraceDemo.method2(TestprintStackTraceDemo.java:21)
    at TestprintStackTraceDemo.method1(TestprintStackTraceDemo.java:17)
    at TestprintStackTraceDemo.main(TestprintStackTraceDemo.java:8)
```
**3.throws/throw关键字**
+ throws是方法可能抛出异常的声明。(用在声明方法时，表示该方法可能要抛出异常)throws关键字放在方法签名的尾部。
+ throw是语句抛出一个异常。

示例：
```
public class className {
    public static void main(String args[]) throws ArrayIndexOutOfBoundsException{
        throw new ArrayIndexOutOfBoundsException();
    }
}
```
**4.finally**
finally关键字用来创建在try代码块后面执行的代码块。无论是否发生异常，finally代码块中的代码总会被执行。在finally代码块中，可以运行清理类型等收尾善后性质的语句。finally代码块出现在catch代码块最后，
> try{
    // 程序代码
 }catch(异常类型1 异常的变量名1){
    // 程序代码
 }catch(异常类型2 异常的变量名2){
    // 程序代码
 }finally{
    // 程序代码
 }

**Notice:**
 + catch不能独立于try存在。
 + 在try/catch后面添加finally块并非强制性要求的。
 + try代码后不能既没catch块也没finally块。
 + try, catch, finally块之间不能添加任何代码。


## 自定义异常类
在Java中你可以自定义异常。编写自己的异常类时需要记住下面的几点。
+ 所有异常都必须是Throwable的子类。
+ 如果希望写一个检查性异常类，则需要继承Exception类。
+ 如果你想写一个运行时异常类，那么需要继承RuntimeException 类。

示例
自定义异常类
```
class MyException extends Exception {
    MyException() {
    }
    MyException(String msg) {
        //调用父类的方法
        super(msg);
    }
}
```
使用异常类
```
public class TestMyException {
    public static void main(String args[ ])
    {
        try
        {
            String level=null;
            level=scoreLevel(90);
            System.out.println("90 分的成绩等级为："+level);
            level=scoreLevel(120);
            System.out.println("120 分的成绩等级为："+level);
        }
        catch(MyException e)
        {
            e.printStackTrace();
        }
    }
    static String scoreLevel(int score) throws MyException
    {
        if(score>=85&&score<=100)
            return "A";
        else if(score>=75&&score<85)
            return "B";
        else if(score>=60&&score<75)
            return "C";
        else if(score<60&&score>=0)
            return "D";
        else
            throw new MyException("非法的分数");
    }
}
```



## 通用异常
在Java中定义了两种类型的异常和错误。
+ JVM(Java虚拟机)异常：由JVM抛出的异常或错误。例如：NullPointerException类，ArrayIndexOutOfBoundsException类，ClassCastException类。
+ 程序级异常：由程序或者API程序抛出的异常。例如IllegalArgumentException类，IllegalStateException类。


我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！
