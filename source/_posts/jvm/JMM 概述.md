---
title: JMM 概述
summary: JMM 概述
categories: JMM
date: 2022-07-01 12:01:00
tags:
- java
---

## JMM 概述

### 硬件底层 Cache 一致性协议

MSI

MESI 

MOSI

Synapse Firefly Dragon


总线锁

缓存行锁

缓存行 64 KB

伪共享

位于同于缓存行的两个不同数据，被两个不同CPU锁定，产生互相影响的伪共享问题

缓存行对齐， padding


volatile 实现细节

JVM 层面

StoreStoreBarrier
volatile 写操作
StoreLoadBarrier

LoadLoadBarrier
volatile 读操作
LoadStoreBarrier

### 常量池 (constant pool table)


int a = 0;

- 字面量 (Literal)     0

字符串常量池

- 1.6及之前，有永久代，运行时常量池在永久代，运行时常量池包含字符串常量池。

- 1.7，有永久代，但逐步去永久代，字符串常量池从永久代分离到堆里

- 1.8及之后，无永久代。运行时常量池在元空间，字符串常量池里依然在堆里


String s1 = "zhangsan";

会在字符串常量池中查找是否存在 "zhangsan"

存在则直接引用，不存在则新建。


String s1 = new String("zhangsan");

- 会在字符串常量池中查找是否存在 "zhangsan"，不存在则新建。

- 会在堆中建立对象，包含字符串 "zhangsan"。

intern 方法

String s1 = new String("zhangsan");

String s2 = s1.intern();

intern 是 native

![intern 方法](/medias/Java/JVM/1657185400329.png)

s2 指向的是常量池中的 "zhangsan"

前提是 常量池中存在 "zhangsan"


String a = "a";

String b = "b";

String c = "c";

String s1 = a + b + c;

StringBuilder s = new StringBuilder();

s.append(a).append(b).append(c);

String s2 = s.toString();

- 符号引用 (Symbolic Reference)  a












