---
title: Java 内存模型（JMM）
summary: 程序 进程 线程
categories: 多线程
date: 2022-05-02 06:25:00
tags:
- 线程
- java
---
## JMM
### java内存模型

Java 内存模型

JMM（Java Memory Model）：Java 内存模型，是 Java 虚拟机规范中所定义的一种内存模型，Java 内存模型是标准化的，屏蔽掉了底层不同计算机的区别。也就是说，JMM 是 JVM 中定义的一种并发编程的底层模型机制。

JMM 定义了线程和主内存之间的抽象关系：线程之间的共享变量存储在主内存中，每个线程都有一个私有的本地内存，本地内存中存储了该线程以读/写共享变量的副本。

JMM 的规定：
- 所有的共享变量都存储于主内存。这里所说的变量指的是实例变量和类变量，不包含局部变量，因为局部变量是线程私有的，因此不存在竞争问题。

每一个线程还存在自己的工作内存，线程的工作内存，保留了被线程使用的变量的工作副本。
线程对变量的所有的操作（读，取）都必须在工作内存中完成，而不能直接读写主内存中的变量。
不同线程之间也不能直接访问对方工作内存中的变量，线程间变量的值的传递需要通过主内存中转来完成。
![JMM](https://isblog.oss-cn-guangzhou.aliyuncs.com/v2-3d312429710bd6a11eca171858f67751_720w.jpg)


### Program Counter

the address (location) of the instruction being executed at the current time.

每个Java虚拟机都有独立的pc。


### Direct Memory
1.4 增加
JVM可以直接访问的内核空间内存，操作系统管理的内存。
NIO,提高效率，实现zero copy

### Heap
虚拟机线程共享的堆
运行时数据区，保存对象实例以及数组。

### Method Area
虚拟机线程共享的方法区
存储class的结构

具体实现
Perm Space(< 1.8)
  字符串常量位于Perm Space
  不会触发FGC清理
Meta Space(>= 1.8)
  字符串常量位于堆
  会触发FGC清理


### JVM stack

每个java虚拟机线程都有独立的JVM stack，与线程创建时间一致。

JVM stack 存储 frames （栈桢）
``` java
    public static void staticMethod(int k){
        
    }
```
静态方法，局部变量表 k
![staticmethod](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/staticmethod.png)

https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/normalmethod.png

``` java
    public void method(int k){
        
    }
```
![method](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/normalmethod.png)
局部变量表 this , k 


``` java
    public static void main(String[] args) {
        JMM jmm = new JMM();
        jmm.method();

        // 0 new #2 <cn/JMM>  在堆中建立对象
        // 3 dup              把对象的地址放到栈中
        // 4 invokespecial #3 <cn/JMM.<init> : ()V>  执行默认构造方法
        // 7 astore_1         弹出对象地址，赋值给 jmm
        // 8 aload_1          把jmm压栈
        // 9 invokevirtual #4 <cn/JMM.method : ()V>  执行method方法
        // 12 return
        
    }

    public void method(){

    }
```

![method](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/returnnull.png)

``` java
  public static void main(String[] args) {
        JMM jmm = new JMM();
        jmm.method();

        // 0 new #2 <cn/JMM>  在堆中建立对象
        // 3 dup              把对象的地址放到栈中
        // 4 invokespecial #3 <cn/JMM.<init> : ()V>  执行默认构造方法
        // 7 astore_1         弹出对象地址，赋值给 jmm
        // 8 aload_1          把jmm压栈
        // 9 invokevirtual #4 <cn/JMM.method : ()V>  执行method方法
        // 12 pop   弹出method栈的的数据，压栈到main栈
        // 13 return

    }

    public int method(){
        return 100;
    }
```
![method](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/return100.png)


``` java
  public static void main(String[] args) {
        JMM jmm = new JMM();
        int r = jmm.method();
        
        // 0 new #2 <cn/JMM>  在堆中建立对象
        // 3 dup              把对象的地址放到栈中
        // 4 invokespecial #3 <cn/JMM.<init> : ()V>  执行默认构造方法
        // 7 astore_1         弹出对象地址，赋值给 jmm
        // 8 aload_1          把jmm压栈
        // 9 invokevirtual #4 <cn/JMM.method : ()V>  执行method方法
        // 12 istore_2       弹出main栈元素，并赋值给 r
        // 13 return
    }

    public int method(){
        return 100;
    }
```
![method](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/return100_i.png)

#### Frame 每个方法对应一个栈帧
 A frame is used to store data and partial results, as well as to perform dynamic linking,return values for method ,and dispatch exceptions.


##### Local Variable Table

##### Operate Stack

##### Dynamic Linking
   
##### return address
返回值地址

方法A调用了方法B，方法B返回值的地址。


### Run-Time Constant Pool
class文件的常量池表


### Native Method Stacks
C++等原生方法对应的栈


![i++](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/iplusplus.jpg)

