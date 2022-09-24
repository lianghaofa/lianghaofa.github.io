---
title: volatile
summary: volatile
categories: 并发
tags:
- 并发
- java
---

## volatile



![JMM](https://isblog.oss-cn-guangzhou.aliyuncs.com/v2-3d312429710bd6a11eca171858f67751_720w.jpg)

### 工作内存与内存保持一致性

volatile 后会添加： lock 前缀指令

### 禁止指令重排序  是否存在存在依赖关系
as-if-serial 语义
// 此情况下存在依赖关系，不会发生重排序
a = y
x = a
// 此情况下不存在依赖关系，可能会发生重排序
a = y
x = y

// 如何判断是否存在依赖关系。编译时的语义分析

happens-before 原则

内存屏障

LoadLoad屏障

StoreStore屏障

LoadStore屏障

StoreLoad屏障

volatile 修饰会添加内存屏障

java规范
a = 2; volatile写， a 为 volatile变量
StoreStore屏障
a = 1; volatile写
StoreLoad屏障
b = a; volatile读
LoadLoad屏障

JVM 屏障的实现 lock前缀




![锁升级](https://isblog.oss-cn-guangzhou.aliyuncs.com/lockUpdateGrade.png)

### 面试题




