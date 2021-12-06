---
title: java控制流程语句
tags: [java基础]
date: 2016-03-29 10:03:31
description: java控制流程语句
---

Java 主要的控制语句有3 种，选择语句、循环语句、跳转语句。
<!-- more -->

## 条件语句
 + `if(){}else{}`
```
int a = 0;
int b = 1;
if(a>b){
    System.out.println("a");
}else{
    System.out.println("b");
};
```
 + `if(){}else if(){}else{}`
```
if(a>b){
    System.out.println("a");
}else if(a<b){
    System.out.println("b");
}else{
    System.out.println("相等");
};
```
 + `switch`
```
int k = 3;
switch(k) {
    case 1:
        System.out.println(1);
    case 2:
        System.out.println(2);
    case 3:
        System.out.println(3);
}
```

## 循环语句

+ `while`
```
int a = 0;
while (a<5){
    System.out.println(a);
    a++;
};
```
+ `do...while`
```
do{
    System.out.println(a);
    a++;
}while(a<5);
```
+ `for`
```
for(a=0 ;a<5;a++){
    System.out.println(a);
}
```
## 跳转语句
+ `break`
```
for(int i=0;i<50;i++) {
    System.out.println("i="+i);
    //当n 等于10 的时候，跳出循环语句
    if(i==10)
        break;
};
```

+ `continue`
```
for (int i = 1; i <51; i++) {
    System.out.print(i+" ");
    if(i%5!=0)
        //当n 不能整除5 的时候继续进行循环
        continue;
    else
        System.out.println("*****");
};
```

+ `return`

```
for(int i=0;i<10;i++)
    if(i<5)
        System.out.println("第"+i+"次循环");
    else if(i==5)
        return;
        //下面的语句永远不会执行
    else
        System.out.println("第"+i+"次循环");

```






我的博客:[http://bigdots.github.io](http://bigdots.github.io)、[http://www.cnblogs.com/yzg1/](http://www.cnblogs.com/yzg1/)



如果觉得本文不错的话，帮忙点击下面的推荐哦,谢谢！