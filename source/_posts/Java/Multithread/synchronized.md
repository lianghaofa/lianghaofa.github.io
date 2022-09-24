---
title: synchronized
summary: synchronized
categories: 并发
tags:
- 并发
- java
---

## synchronized
### CAS

ABA问题？
Sting a = A;
线程1，读取a = A 
线程2,操作 a = B, a = A
线程1，操作 a = B, compare a ，虽然一致，但此过程已经发生过修改。
//
如何知道a 是否发生过修改？
版本控制，修改一次 version ++


compare and swap 底层实现
单核CPU cmpxchg  指令

多核CPU lock 和 cmpxchg  指令

lock 指令执行后，lock 缓存行
数据跨多个缓存行，lock总线。

CPU一般都是多核。
CAS最终还是加锁，只是粒度更小。

jdk早期，synchronized是重量锁，jvm申请内核态的锁，需要用户态转内核态，内核态转用户态，因为申请锁必须通过kernel系统调用。

CAS 的实现
原子类的操作
AtomicInteger atomicInteger = new AtomicInteger();
AtomicBoolean atomicBoolean = new AtomicBoolean();

### 分段CAS  LongAdder



![LongAdder](https://isblog.oss-cn-guangzhou.aliyuncs.com/longAdress.jpg)



### 对象的内存布局-hotspot
markword 8字节
classpointer 4字节 此对象是哪个类  
成员变类m  int 4字节
8字节对齐

``` java
    public class ObjectLayOut {
    
    static class MyObject{
        int m = 1;
    }

    public static void main(String[] args) {
        Object o = new Object();
        System.out.println(ClassLayout.parseInstance(o).toPrintable());

        MyObject o1 = new MyObject();

        System.out.println(ClassLayout.parseInstance(o1).toPrintable());
    }
}
```
OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
0     4        (object header)                           01 00 00 00 (00000001 00000000 00000000 00000000) (1)
4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
8     4        (object header)                           e5 01 00 f8 (11100101 00000001 00000000 11111000) (-134217243)
12     4        (loss due to the next object alignment)
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total

**12字节 + 4字节(字节对齐)**

cn.ObjectLayOut$MyObject object internals:
OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
0     4        (object header)                           05 00 00 00 (00000101 00000000 00000000 00000000) (5)
4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
8     4        (object header)                           d0 cb 00 f8 (11010000 11001011 00000000 11111000) (-134165552)
12     4    int MyObject.m                                1
Instance size: 16 bytes
Space losses: 0 bytes internal + 0 bytes external = 0 bytes total

**16字节 + 0字节(字节对齐)**

``` java
    public class ObjectLayOut {
    
    static class MyObject{
        int m = 1;
    }

    public static void main(String[] args) {
        Object o = new Object();
        System.out.println(ClassLayout.parseInstance(o).toPrintable());

        MyObject o1 = new MyObject();

        System.out.println(ClassLayout.parseInstance(o1).toPrintable());
    }
}
```
java.lang.Object object internals:
OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
0     4        (object header)                           50 f8 ec 02 (01010000 11111000 11101100 00000010) (49084496)
4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
8     4        (object header)                           e5 01 00 f8 (11100101 00000001 00000000 11111000) (-134217243)
12     4        (loss due to the next object alignment)
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total

cn.ObjectLayOut$MyObject object internals:
OFFSET  SIZE   TYPE DESCRIPTION                               VALUE
0     4        (object header)                           05 b0 ed 02 (00000101 10110000 11101101 00000010) (49131525)
4     4        (object header)                           00 00 00 00 (00000000 00000000 00000000 00000000) (0)
8     4        (object header)                           d0 cb 00 f8 (11010000 11001011 00000000 11111000) (-134165552)
12     4    int MyObject.m                                1
Instance size: 16 bytes
Space losses: 0 bytes internal + 0 bytes external = 0 bytes total

what's the different ?
**markword**

![markword](https://isblog.oss-cn-guangzhou.aliyuncs.com/1117548-20200401000907595-1715070682.png)

### 锁升级
![锁升级](https://isblog.oss-cn-guangzhou.aliyuncs.com/1117548-20200401000058814-2083145528.png)


轻量级锁：自旋锁

默认：轻量级锁
### 偏向锁只要有竞争就会升级为轻量级锁（自旋锁）
偏向锁，轻量级锁 用户态
重量级锁  内核态

没有竞争，偏向锁效果最好。
明确知道有多线程竞争的情况下，此时应直接使用自旋锁。
JVM启动过程，会自动启动多线程，默认情况启动会关闭偏向锁。

new Object();创建新对象。
偏向锁未启动：001。
``` java
    public static void main(String[] args) {
        // JVM启动过程，会自动启动多线程，默认情况启动会关闭偏向锁。
        Object o = new Object();

        // 偏向锁未启动
        System.out.println(ClassLayout.parseInstance(o).toPrintable());

        Thread.sleep(5000);

        // 匿名偏向锁
        System.out.println(ClassLayout.parseInstance(o).toPrintable());

        // 匿名偏向锁成为偏向锁 
        synchronized (o){
            System.out.println(ClassLayout.parseInstance(o).toPrintable());
        }
    }
}
```
偏向锁启动：101,指向00000，匿名偏向。

对对象上锁，匿名偏向锁成为偏向锁。

偏向锁 调用wait方法，直接升级重量级锁。
偏向锁 重度竞争，升级重量级锁。


偏向锁，轻量级锁 用户态
重量级锁  内核态


thread1没有竞争，直接获取偏向锁
遇到竞争，thread2
两者自旋竞争
thread1, lockrecord
thread2, lockrecord

如果thread1成功
thread2自旋等待。
![锁升级](https://isblog.oss-cn-guangzhou.aliyuncs.com/synchronized_update.png)

![锁升级](https://isblog.oss-cn-guangzhou.aliyuncs.com/lockUpdateGrade.png)

###锁重入
synchronized 是可重入锁
需要记录重入次数，解锁需要对应的次数。
偏向锁 自旋锁 记录在线程栈->LR

重量级锁：
ObjectMonitor字段上

### 自旋锁升级重量锁锁
自旋超10次，或自旋线程超过CPU核数一半。
1.6之后加入自旋Adapative Spining,JVM自己控制。

自旋等待占用CPU资源，如果锁的时间长，自旋线程多，CPU会被大量消耗。
此时会升级重量级锁。

重量级如何解决此为题？
把大量的自旋线程冻结在队列中。

synchronized
**使用对象加锁，切记不要把引用指向其他对象**
```java
    public static class T {
        Object o = new Object();       
        void m(){
            synchronized (o){
                while (true){
                    try {
                        Thread.sleep(1000);
                    }catch (Exception e){
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName());
                }
            }
        }
    }
    
    public static void main(String[] args) throws InterruptedException {
       
        T t = new T();
        new Thread(t :: m, "t1").start();
        try {
            Thread.sleep(5000);
        }catch (Exception e){
            e.printStackTrace();
        }
        
        Thread t2 = new Thread(t::m, " t2");
        // wrong!!!
        t.o = new Object();
        t2.start();
    }
```

**final Object o = new Object();**


```java
    //  increase0 相当于锁了调用当前方法的对象
    int sum = 0;
    public synchronized void increase0(){
        sum  ++;
    }

    public void increase1(){
        synchronized (this){
            sum  ++;
        }
    }
```

### synchronized 底层逻辑

1.6之前  monitor对象  加锁


字节码层面

ACC_SYNCHRONIZED

monitorenter

monitorexit

JVM层面

C C++ 调用操作系统的同步机制

OS 和 硬件层面

X86 lock comxchg















