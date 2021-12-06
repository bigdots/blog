---
title: Java线程
tags: [Java基础]
date: 2016-04-12 14:06:05
description: Java线程
---

## 线程
1. 线程
> 线程（Thread）是控制线程（Thread of Control）的缩写，是程序运行的基本单位，它是具有一定顺序的指令序列（即所编写的程序代码）、存放方法中定义局部变量的栈和一些共享数据。线程是相互独立的，每个方法的局部变量和其他线程的局部变量是分开的，因此，任何线程都不能访问除自身之外的其他线程的局部变量。如果两个线程同时访问同一个方法，那每个线程将各自得到此方法的一个拷贝。

<!-- more -->

2. 多线程
> 多进程：多线程实现后台服务程序可以同时处理多个任务，并不发生阻塞现象。多线程是Java 语言的一个很重要的特征。多线程程序设计最大的特点就是能够提高程序执行效率和处理速度。Java 程序可同时并行运行多个相对独立的线程。
3. 进程
> 进程：一个进程包括由操作系统分配的内存空间，包含一个或多个线程。一个线程不能独立的存在，它必须是进程的一部分。一个进程一直运行，直到所有的非守候线程都结束运行后才能结束。


## 实现
Java提供了两种实现线程的方法
1. 实现Runnable接口
Runnable 接口提供了run()方法的原型，因此创建新的线程类时，只要实现此接口，即只要特定的程序代码实现Runnable 接口中的run()方法，就可完成新线程类的运行。
 + 创建一个实现Runnable 接口的类，并且在这个类中重写run 方法。
 > class ThreadType implements Runnable{
    public void run(){
        ……
    }
}
 + 使用关键字new 新建一个ThreadType 的实例
 > Runnable threadName = new ThreadType ();
 + 创建线程（threadOb 是一个实现Runnable 接口的类的实例；threadName指定新线程的名字）
 > Thread(Runnable threadOb,String threadName);
 + 启动线程
 > threadName.start();

实例：
```
public class RunnableDemo implements Runnable {
    public void run(){
        for (int count = 1,row = 1; row < 10; row++,count++) //循环计算输出的*数目
        {
            for (int i = 0; i < count; i++) //循环输出指定的count 数目的*
            {
                System.out.print('*'); //输出*
            }
            System.out.println(); //输出换行符
        }
    }

    public static void main(String argv[ ]){
        Runnable rb = new RunnableDemo(); //创建，并初始化RunnableDemo 对象rb
        Thread td = new Thread(rb); //通过Thread 创建线程
        td.start(); //启动线程td
    }

}
```
2. 派生Thread类
继承Thread 类并覆盖Thread 类的run 方法完成线程类的声明，通过new 创建派生线程类的线程对象。
 + 创建一个新的线程类，继承Thread 类并覆盖Thread 类的run()方法
 > class ThreadType extends Thread{
     public void run(){
       ……
     }
}
 + 创建线程
 > ThreadDemo threadName = new ThreadDemo();
 + 启动线程
 > threadName.start();

实例：
```
public class ThreadDemo extends Thread {
    //重载run 函数
    public void run() {
        for (int count = 1,row = 1; row < 10; row++,count++) //循环计算输出的*数目
        {
            for (int i = 0; i < count; i++) //循环输出指定的count 数目的*
            {
                System.out.print('*'); //输出*
            }
            System.out.println(); //输出换行符
        }
    }
    public static void main(String argv[ ]){
        ThreadDemo td = new ThreadDemo(); //创建，并初始化ThreadDemo类型对象td
        td.start(); //调用start()方法执行一个新的线程
    }
}
```

## 线程周期

一个线程有 4 种状态，任何一个线程都处于这4 种状态中的一种状态。
+ 创建（new）状态：调用new 方法产生一个线程对象后、调用start 方法前所处的状态。线程对象虽然已经创建，但还没有调用start 方法启动，因此无法执行。当线程处于创建状态时，线程对象可以调用start 方法进入启动状态，也可以调用stop 方法进入停止状态。
+ 可运行（runnable）状态：当线程对象执行start()方法后，线程就转到可运行状态。进入此状态只是说明线程对象具有了可以运行的条件，但线程并不一定处于运行状态。因为在单处理器系统中运行多线程程序时，一个时间点只有一个线程运行，系统通过调度机制实现宏观意义上的运行线程共享处理器。因此一个线程是否在运行，除了线程必须处于Runnable 状态之外，还取决于优先级和调度。
+ 不可运行（non Runnable）状态：线程处于不可运行状态是由于线程被挂起或者发生阻塞，例如对一个线程调用wait()函数后，它就可能进入阻塞状态；调用线程的notify 或notifyAll 方法后它才能再次回到可执行状态。
+ 退出（done）状态：一个线程可以从任何一个状态中调用stop 方法进入退出状态。线程一旦进入退出状态就不存在了，不能再返回到其他的状态。除此之外，如果线程执行完run 方法，也会自动进入退出状态。
来看菜鸟教程的一张图：
![](/images/201601/process.png)


## 线程状态转换函数
![](/images/201601/state.png)
注意：stop()、suspend()和resume()方法现在已经不提倡使用，这些方法在虚拟机中可能引起“死锁”现象。suspend()和resume()方法的替代方法是wait()和sleep()。线程的退出通常采用自然终止的方法，建议不要人工调用stop()方法。



















我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！