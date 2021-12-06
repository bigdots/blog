---
title: Java数组
tags: [java基础]
date: 2016-03-29 13:37:06
description: Java数组
---
> Java 语言中的数组是用来存放同一种数据类型数据集的特殊对象。

<!-- more -->

## 数组基础
**1.声明数组**
+ ArrayType ArrayName[ ];
```
int array1[ ];
```

+ ArrayType [ ] ArrayName;
```
int [ ] array2,array3;
```

*这两种声明方式没有区别，但是第二种可以同时声明多个数组，使用起来较为方便*


**2.分配空间**
ArrayName = new  ArrayType [ ArrayLenth ];
```
array1=new int [5];
```

**3.初始化**
+ ArrayName[i] = value
```
array1[0]=1;
array1[1]=2;
array1[2]=3;
array1[3]=4;
array1[4]=5;
```
+ ArrayType [] ArrayName = {value...}
```
int [] array1={1,2,3,4,5};
```

**4.length**
Java 中的数组是一种对象，它会有自己的实例变量。数组只有一个公共实例变量，也就是length 变量，这个变量指的是数组的长度。

## 数组的使用
**1.复制数组**
+ array1=array2;
两个数组类型变量都指向同一个数组array2；
```java
int [] array1 = {1,2,3,4,5};
int [] array2 = {1,2,3,4,5,6};
array1 = array2;
for(int i=0;i<array1.length;i++){
    System.out.println(array1[i]);//123456
}
array2[0] = 8;
System.out.println(array1[0]);//8,说明数组1指向了数组2
```

+ System.arraycopy(fromArray ,fromIndex,toArray,toIndex,length)
从指定源数组fromArray 中复制一个数组，复制从指定的位置fromIndex 开始，到目标数组toArray，在指定位置toIndex 结束，复制length 个元素,注意目标数组必须有足够的空间来存放复制的数据，如果空间不足的话，会抛出异常，并且不会修改该数组。

```
int [] array3 = {1,2,3,4,5};
int [] array4 = {1,2,3,4,5,6};
System.arraycopy(array4 ,0,array3,2,3);
for(int i = 0;i<array3.length;i++){
    System.out.println(array3[i]);//12123
}
System.arraycopy(array4 ,0,array3,2,3);//error,空间不足，抛出异常
```

## 数组排序
**1.选择排序**
选择排序的基本思路是：对一个长度为n 的数组进行n 趟遍历，第一趟遍历选出最大（或者最小）的元素，将之与数组的第一个元素交换；然后进行第二趟遍历，再从剩余的元素中选出最大（或者最小）的元素，将之与数组的第二个元素交换。这样遍历n 次后，得到的就为降序（或者升序）数组。

**2.冒泡排序**
冒泡排序的过程，是把数组元素中较小的看作是“较轻”的，对它进行“上浮”操作。从底部开始，反复地对数组进行“上浮”操作n 次，最后得到有序数组。

**3.快速排序**
通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

## 多维数组
多维数组类似于空间表示中的一维空间、二维空间、多维空间。Java 是支持多维数组的，并利用多个下标来表示多维
**1.声明**
int [ ][ ]twoD=new int[5][5];
上面的语句声明了一个5 行5 列的二维数组

**2.数组的初始化**
+ 直接赋值法
```
twoD={
{1,2,3,4,5},
{6,7,8,9,10},
{11,12,13,14,15},
{16,17,18,19,20},
{21,22,23,24,25}
};
```
+ 使用循环访问数组的每个元素的方法
```
for(int i=0;i<twoD2.length;i++)
for(int j=0;j<twoD2[i].length;j++)
twoD2[i][j]=k++;
```
*twoD2.length 表示的是数组的行数，而twoD2[i].length 表示的则是数组的列数*

> 在 Java 中实际上只有一维数组，多维数组可看作是数组的数组。例子中，二维数组twoD 的实现是数组类型变量指向一个一维数组，这个数组有5 个元素，而这5个元素都是一个有5个整型数的数组

我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！