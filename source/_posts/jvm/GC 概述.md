---
title: GC 概述
summary: GC 概述
categories: GC
date: 2022-09-11 12:01:00
tags:
- GC
---

## GC 概述


![java 堆](/medias/Java/JVM/1663641983801.png)

JDK1.8 PermGen被MetaData代替，为什么？

![PermGen被MetaData代替](/medias/Java/JVM/1663646305190.png)
PermGen不知道该分配多少，分多了浪费空间，分少了堆溢出OOM。MetaData可以增长，但也有最大值限制。

如何解决线上GC频繁的问题？

找出问题。

修改了某一模块后出了问题，先定位了此模块。检查代码


jvm有哪些垃圾回收器，实际中如何选择？

从单线程到多线程，到G1

多线程不能继续STOP THE WORLD.开发新的垃圾收集器，CMS


吞吐量优先 - Parallel Scavenge + Parallel Old 多线程并行

响应时间优先 - CMS + ParNew 并发垃圾收集


- 什么叫阻塞队列的有界和无界，实际中有用过吗？

阻塞队列

ArrayBlockingQueue:一个由数组结构组成的有界阻塞队列，由MAXLEN作为限制，一把锁

LinkedBlockingQueue:一个由链表结构组成的无界阻塞队列，由内存作为限制，两把锁

PriorityBlockingQueue:堆的数据结构 + Comparator接口

SynchronousQueue:一个不存储元素的队列，线程池 ? 有瑕疵 ？

LinkedTransferQueue:

LinkedBlockingQueue:一个由链表结构组成的双向无界阻塞队列









