---
title: AQS
summary: Thread
categories: 并发
tags:
- AQS
- java
---

## Thread
### 类方法


| 方法      | 说明 |
| :----: | :----: |
| start()      | 启动线程       |
| setName(String name)   | 设置线程名称        |
| setPriority(int priority)     | 设置线程优先级，默认5，取值1-10       |
| join(long millisec)   | 挂起线程xx毫秒，参数可以不传        |
| interrupt()      | 终止线程       |
| isAlive()   | 测试线程是否处于活动状态        |

### 静态（static）方法

| 方法      | 说明 |
| :----: | :----: |
| yield()      | 暂停当前正在执行的线程对象，并执行其他线程。       |
| sleep(long millisec)/sleep(long millis, int nanos)   | 挂起线程xx秒，参数不可省略        |
| currentThread()      | 返回对当前正在执行的线程对象的引用       |
| holdsLock(Object x)   | 当前线程是否拥有锁        |













