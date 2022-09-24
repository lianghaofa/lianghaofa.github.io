---
title: JVM 概述
summary: JVM 概述
categories: JVM
date: 2022-09-20 12:01:00
tags:
- java
---

## JVM 概述


简单来说就是堆 + 栈 + PC + 其他。

栈 - 虚拟机栈、本地方法栈

堆 - 申请的内存，new

其他 - 老版本叫Method Area,新版本的元数据区、线程共享的数据区、线程隔离的数据区


java执行引擎 - 编译解释字节码，垃圾回收

![java执行引擎](/medias/Java/JVM/10618330ffb7352a0e4a79a3112d042.png)

解释器 - C++解释java文件，汇编解释java文件

即时编译器 - 中间代码生成器、代码优化器、目标代码生成器、探测分析器








