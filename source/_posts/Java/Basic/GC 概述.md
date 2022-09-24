---
title: GC 概述
summary: Garbage Collection
categories: GC 概述
date: 2022-07-05 09:25:00
tags:

---
## GC 概述



![JVM](/medias/Java/JVM/1657077980216.png)


线程栈 - 栈帧

本地方法栈 - native 方法 

Reference Count 引用计数

Root Searching 

GC roots 

- 线程栈变量  - 例如 main 方法中的变量

- 静态变量

- 常量池

- JNI指针 c c++ 方法

which instance are roots?

- JVM stack

- native method stack

- run-time constant pool

- static references in method area

- class

### 对象内存回收

#### 引用计数法


实现简单，效率高，但难以解决对象之间相互循环引用的问题。


![引用计数法 - 循环引用](/medias/Java/JVM/1657285774264.png)


##### 强引用

User user = new User();



##### 软引用

SoftReference<User> user = new SoftReference(new User());

GC 空间不够，会清除

##### 弱引用

WeakReference<User> user = new WeakReference(new User());


GC 会直接回收

##### 虚引用




##### finalize

1.第一次标记并进行一次筛选

如果没有 finalize 方法，直接回收


2.第二次标记

finalize


#### 可达性分析法







### GC 常用算法

#### Mark-Sweep (标记清除)

碎片


两遍扫描

- 根据 GC Roots 确定不可收的区域

- 可回收的区域放到空闲列表中


![Mark-Sweep](/medias/Java/JVM/1656992111094.png)
#### Copying (拷贝)

分区


![Copying](/medias/Java/JVM/1656992364777.png)

优缺点

![Copying - 优缺点](/medias/Java/JVM/1656992442730.png)

#### Mark-Compact (标记压缩)

![Mark-Compact](/medias/Java/JVM/1656993080975.png)


![Mark-Compact 优缺点](/medias/Java/JVM/1656993193239.png)

### 常见垃圾回回收器







Epsilon ZGC Shenandoah 都在使用逻辑分代模型

G1是逻辑分代，物理不分代

除此之外不仅逻辑分代，而且物理分代


![堆内存逻辑分区](/medias/Java/JVM/1656994342250.png)

Eden 回收后，存活的对象移动到Survivor

S0 S1分区，是copy算法。

复制次数超多一定次数，进入Old区。

Eden ->  Survivor0

Eden 、 Survivor0 -> Survivor1


新生代 - copy 算法

老年代 - Mark-Sweep (标记清除)




![垃圾回收时机](/medias/Java/JVM/1657007945234.png)



大对象 - 需要指定某些垃圾收集器

-XX:PretenureSizeThreshold= 10000 (Byte)

-XX:+UseSerialGC





![内存分配](/medias/Java/JVM/1657009760684.png)


![对象如何进入老年代](/medias/Java/JVM/1657011560707.png)




![java - 内存分配](/medias/Java/JVM/1657012218337.png)


动态年龄

年龄1 + 年龄2 + 年龄3 + .. 年龄i .. + 年龄n


年龄1 + 年龄2 + 年龄3 + .. 年龄i >= Survivor, 年龄i + 1 ... 年龄n放到老年代中。

分配担保

YGC 期间 survivor 区空间不够了 空间担保直接进入老年代




![GC](/medias/Java/JVM/1657013502940.png)


 Serial 单线程

Serial Old


![GC 组合](/medias/Java/JVM/1657013679420.png)



#### Serial - Serial Old


![Serial - 串行](/medias/Java/JVM/1657014740505.png)

1657014740505.png

找安全点进行垃圾回收

safe point 


内存空间小，垃圾回收的时间短。内存空间较大，耗时较长


Serial Old - 针对 Old 区


![Serial Old - 串行](/medias/Java/JVM/1657014977552.png)



#### Parallel Scavenge - Parallel old

默认 PS + PO


![Parallel Scavenge](/medias/Java/JVM/1657027484311.png)


![Parallel old](/medias/Java/JVM/1657027822118.png)



#### ParNew - CMS (老年代垃圾回收器)

ParNew


CMS - concurrent mark sweep 

STP 方式对于大内存，耗时较长，必须得有新的垃圾回收方式。

CMS 之前  垃圾回收时，其他线程都得停止运行。


- 初始标记(initial mark) - 对 Garbage Roots 进行标记。

![初始标记(initial mark)](/medias/Java/JVM/1657031248187.png)

- 并发标记(concurrent mark) - 80% 时间消耗在此。 垃圾回收线程与工作线程同时工作。

![并发标记(concurrent mark)](/medias/Java/JVM/1657032040203.png)

并发标记(concurrent mark) 算法

G1 三色标记从+ SATB

ZGC 颜色指针 + 写屏障

Shenandosh  颜色指针 + 读屏障


- 重新标记(remark) - 只有垃圾回收线程工作，因为并发标记阶段已经把大部分的垃圾做了标记。

![重新标记(remark)](/medias/Java/JVM/1657032221217.png)

- 并发清理(concurrent sweep) 

![并发清理(concurrent sweep)](/medias/Java/JVM/1657032282694.png)

Concurrent model failure , 此时会进入 stop the world, serial old 来进行回收。

CMS的问题

- Memory Fragmentation  碎片化

- Floating Garbage  

垃圾收集器跟内存大小的关系

1.Serial - 几十兆
2.PS - 上百兆
3.CMS - 20G
4.G1 - 上百G
5.ZGC - 4T


内存泄露 Memory leak  - 无法回收 ？

内存溢出 out of memory


CMS GC日志



![ParNew](/medias/Java/JVM/1657447261140.png)


![Initial Mark](/medias/Java/JVM/1657447404632.png)






CMS 相关参数

- -XX：CMSInitiatingOccupancyFraction 使用多少比例的老年代后开始CMS收集，默认 68%。



### 分区模型


![分区模型](/medias/Java/JVM/1657350056725.png)

#### G1



CSet = Collection Set

一组可被回收得分区的集合


RSet = RememberedSet

记录其他Region中的对象





Young GC

Mixed GC - CMS ?

XX:InitiatingHeapOccupacyPercent

- 默认值45%

- 当堆内存占用超过此值会触发MixedGC

Mixed GC 流程

- 初始标记 STW

- 并发标记

- 最终标记 STW (重新标记)

- 筛选标记 STW (并行)





Full GC

G1 触发 FGC，如何减免 FGC 的触发 ？

- 扩内存

- 提高CPU性能

- 降低MixedGC触发的阈值，让MixedGC提高发生(默认45%)


G1 java 10以前是串行FullGC,之后是并行FullGC


G1 日志




#### ZGC


-XX:+UseZGC

1.支持 4 TB 级别的堆

2.停顿时间(STW) 不超过10ms, 且不会随着堆的大小增加而增加。


三色指针


![三色指针](/medias/Java/JVM/1657352742452.png)

- 蓝色，初始化，没有可达关系

- 绿色  可达分析标识过

- 红色 可达分析标识过



![ZGC 流程](/medias/Java/JVM/1657359731939.png)
- 初始标记


- 并发标记


- 再标记

- 并发转移准备

- 初始转移

- 并发转移

转发表

读屏障


ZGC 中转移 = copy


三色标记算法


#### Shenandoah


#### Eplison



### 可达性算法


### 并发标记算法

难点： 在标记对象过程中，对象引用关系正在发生改变


#### 三色标记法


![三色](/medias/Java/JVM/1657424991730.png)

白色：未被标记的对象

灰色：自身被标记，成员变量未被标记

黑色：自身和成员变量均已标记完成


漏标

![漏标前](/medias/Java/JVM/1657425243823.png)


![漏标后](/medias/Java/JVM/1657425243823.png)


灰色对象的D指向没了。


黑色对象突然指向了D对象，黑色不然再继续标记。


1.incremental update - 增量更新，关注引用的增加，把黑色重新标记为灰色，下次重新扫描属性(所有对象都需要重新扫描)。    CMS使用该算法




2.SATB snapshot at the beginning - 关注引用的删除。当 B -> D 消失时，要把这个引用压到GC的堆栈，保证D还能被GC扫描到。      G1使用该算法







### 实际 JVM 调优

Jmap 、 Jstack 、 Jinfo

Jmap 查看内存信息，实例个数以及内存大小

- Jmap - history

- Jmap - heap 堆信息

Jstack 查看死锁


jvisualvm


-Xms3072M -Xmx3072M -Xss1M

-XX:MetaspaceSize=512M

-XX:MaxMetaspaceSize=512M






#### GC 常用参数

-Xmn 年轻代

-Xms 堆最小大小

-Xmx 堆最大大小

-Xss 栈空间

-XX:+UseTLAB 使用TLAB，默认打开

-XX:+PrintTLAB 打印TLAB的使用情况

-XX:TLABSize TLAB大小

-XX:+DisableExplictGC System.gc() 失效

-XX:MaxTenuringThreshold 升代年龄，最大值 15 

-XX:PreBlockSpin 锁自旋次数 

-XX:CompileThreshold 热点代码检测参数



Parallel 常用参数

- -XX:SurvivorRatio  默认 8 : 1 : 1

- -XX:PreTenureSizeThreshold  大对象的大小，大于此值，直接进Old区

- -XX:MaxTenuringThreshold

- -XX:+ParallelGCThreads 并行收集器的线程数，同样适用于CMS,一般设为和CPU核数相同

- -XX:+UseAdaptiveSizePolicy 自动选择各区大小比例


CMS 常用参数

- -XX:+UseConcMarkSweepGC  垃圾清除算法

- -XX:ParallelCMSThreads CMS线程数量

- -XX:CMSInitiatingOccupacyFraction 使用多少比例的老年代后开始CMS收集，默认是68%(近似值),如果频繁发生SerialOld卡顿，应该调小

- -XX：+UseCMSCompactAtFullCollection 在FGC时进行压缩

- -XX：CMSFullGCsBeforeCompaction 多少次FGC之后进行压缩

- GCTimeRatio 设置GC时间占程序运行时间的百分比

- XX：MaxGCPauseMilis 停顿时间，只是一个建议时间.



G1 常用参数

- -XX:+UseG1GC 

- -XX:MaxGCPauseMilis    建议值，G1尝试调整Young区的块数来达到这个值

- -XX:GCPauseintervalMilis

- -XX:+G1HeapRegionSize  分区大小

- G1NewSizePercent 新生代最小比例 默认 5%

- G1MaxNewSizePercent 新生代最小比例 默认 60%

- GCTimeRatio GC时间建议

- ConcGCThreads 线程数量

- InitiatingHeapOccupacyPercent 启动G1的堆空间占用比例









#### GC日志


Minor GC 的触发时间

- Eden 空间不足

- 多线程并行执行

Full GC 触发时间

- Old 空间不足

- System.gc()


Minor GC 之前







![Minor GC](/medias/Java/JVM/1657284496753.png)