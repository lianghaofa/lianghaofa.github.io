---
title: Redis 概述
date: 2022-06-01 09:40:52
summary: Redis 概述
categories: Redis
tags:
- Redis 

---
## Redis 概述

MemoryCache - K,V

Redis - K,V

MemoryCache 的 V 只支持单一类型。  

Redis 的 V 支持多种类型。

- strings

- hashes

- lists

- sets

- sorted sets


MemoryCache 的 V 存放单一类型，只要转换后，也可能存放各种数据结构。为何实现了具体类型 redis 更好？

假设，寻找lists 里的index 为 100 的值， redis 只需要把 index = 100 的数据 load 到内存，
但  MemoryCache 需要把该 V 的所有数据load 到内存， 然后进行数据类型的转换。简单的说，如果K,V，V是字符串，那么两者性能差别不大。

Linux - 主要的相关系统调用 

read 

socket() 可阻塞，可不阻塞 

BIO 如果需要读文件，此时阻塞，处理完当前请求，再处理下一个请求

NIO 同步，非阻塞。同步 ？ 如果有1000个请求，有1000个文件描述符，每次都要系统调用， ？？？ 有办法不需要系统调用？  那得需要改内核

多路复用NIO select() 1000个请求，给了系统1000个文件描述符，调用一次系统调用。当某些文件已经读取完成，向用户返回该文件描述符，用户线程可以调用read函数，直接获取数据。

事件驱动
select() bitmap

epoll() 链表 + 红黑树

红黑树 - 存储事件

为什么是红黑树 ？ 因为有些情况下可能需要删除某些事件

链表 - 完成的事件，链表

回调函数，再怎么回调。我调用你的事件，不需要你去监听。内核是否能触发用户的函数

**妈的，你调用完直接返回数据好了，别烦用户了**


mmap 系统调用

减少内核态与用户态的切换，共享空间。 内核态把数据写到共享空间，


sendfile 系统调用

read(fd3),write(fd4)

ZeroCopy - sendfile(out,in)   假设fd4为网卡，fd3 sendfile(fd4,fd3)， 把 fd3 读取完后直接发送到网卡。

例如：
Kafka 生产者 写数据使用 mmap 写，写共享空间

Kafka 消费者 获取数据， 使用 sendfile 读取磁盘数据后直接向相关端口发送数据


02 redis(二) 03 

原子性操作


二进制安全

编码与解码方式的一致

统一存储 字节数组，编码解码交由用户处理。
 



setbit

redis BloomFilter




### Server-assisted, client-side caching


The Redis client-side caching support is called Tracking, and has two modes:

在默认模式下，服务器会记住给定客户端访问的键，并在相同的键被修改时发送无效消息。 这将占用服务器端的内存，但是只会为客户端内存中可能存在的一组键发送无效消息。

1.如果需要，客户端可以启用跟踪。 连接在没有启用跟踪的情况下启动。  
2.当启用跟踪时，服务器会在连接生命周期内记住每个客户端请求的键(通过发送关于这些键的读取命令)。  
3.当某个密钥被某些客户端修改，或者因为它具有关联的过期时间而被驱逐，或者因为maxmemory策略而被驱逐时，所有启用跟踪并可能缓存了该密钥的客户端都会收到无效消息通知。  
4.当客户端接收到无效消息时，他们需要删除相应的键，以避免提供过时的数据。


在广播模式下，服务器不会试图记住给定客户端访问的键，因此这种模式在服务器端根本不消耗任何内存。 相反，客户端订阅诸如object:或user:这样的键前缀，并在每次触摸与订阅的前缀相匹配的键时收到通知消息。

1.服务器将记住可能在单个全局表中缓存了给定键的客户端列表。 该表称为失效表。 失效表可以包含最大数量的表项。 如果插入了一个新的密钥，服务器可能会通过假装这个密钥被修改(即使它没有被修改)来清除旧的条目，并向客户机发送一条无效消息。 这样做，它就可以回收该密钥使用的内存，即使这会迫使拥有该密钥本地副本的客户端将其逐出。  
2.在失效表内部，我们真的不需要存储指向客户端结构的指针，这将在客户端断开连接时强制一个垃圾收集过程:相反，我们所做的只是存储客户端ID(每个Redis客户端都有一个唯一的数字ID)。 如果客户端断开连接，信息将被增量垃圾收集，因为缓存插槽无效。  
3.有一个单键名称空间，不按数据库编号划分。 因此，如果一个客户端正在缓存数据库2中的关键foo，而其他客户端改变了数据库3中的关键foo的值，仍然会发送一条无效消息。 通过这种方式，我们可以忽略数据库编号，从而减少内存使用量和实现复杂性。  




单线程

为啥单线程可以这么快？

单线程指的是处理请求的单线程，IO threads 是多线程，redis 默认 io写 多线程， 不开启读的多线程，读由主线程串行执行。

redis 如何rehash? 如何扩容？

两个数组 dictEntry

100 % N

N组，每组2个存储单元，hash后直接操作单元0，需要扩容的话，利用单元1扩容，扩容完单元0与单元1的指针互换。

扩容期间，新增的写，写到单元1。

读数据时，需要读取两个表。



NoSQL 集群化，如何进行扩容？


网卡IO层获取数据的形式，字节(流)数组。

value 的最长长度。512M？

readQueryFromClient


encoding

1.客户端发来的数据

2.OBJ_STRING 统一封装

3.封装完之后进行 encoding 方式

目的 - 节省存储空间


"9876543210" 存储字符需要10个字节，存储为INT只需要8个字节，64位(操作系统位数)


"98765432100000" 存储字符需要15个字节，存储为INT只需要8个字节，64位(操作系统位数)

OBJ_STRING 0

- OBJ_ENCODING_EMBSTR  内存连续的空间，大小64B， 一个缓存行

- OBJ_ENCODING_RAW  

- OBJ_ENCODING_INT


encoding会在底层自动转换。


OBJ_LIST  1

- OBJ_ENCODING_QUICKLIST -> ZIPLIST

- OBJ_ENCODING_ZIPLIST


OBJ_SET 2

- OBJ_ENCODING_HT

- OBJ_ENCODING_INSET


OBJ_ZSET 3

- OBJ_ENCODING_SKIPLIST

- OBJ_ENCODING_ZIPLIST

数据量小直接使用ZIPLIST

数量量大 ZIPLIST +  SKIPLIST，提高速度



OBJ_HASH 4

- OBJ_ENCODING_ZIPLIST



### SDS - simple dynamic string


1.二进制安全
2.内存预分配，避免频繁的内存分配

char data[] = "guojia";

默认有\0，遇到\0就停止。

socket 接收数据 "guo\0jia";

此时可能导致数据丢失。

SDS

len : 7

char data[] = "guo\0jia";

少于1M，成倍扩容，大于等于1M，每次扩1M


redis 数据存储方式

数组 + 链表

hash(key) -> (非常大的数) % m     redis 默认 16个DB


如何区分value数据类型？ 命令操作名称，get, lpush

缓存行 64字节  

redisObject, 16字节  剩余48字节  ， sds 4 字节，剩余44字节， value 少于等于44字节，直接放 value.  embstr 就是此含义。

如果超过44字节，redis 转换为 raw 类型


sdshdr5 少于等于32字节

type(3b) + len(5b) + buff(32B)

5b表示长度

sdshdr8

8b表示长度

.
.
.

sdshdr64

64b表示长度

redis 6.0 批处理读，批处理读操作？ 读与写分开 ？ 


### list

ziplist - 

quicklist - 双端链表

内存空间有限，尽可能压缩数据空间。

### set


全是数字的情况下，encoding - intset

否则 encoding - hashtable


### zset 有序 set

底层 字典(dict) + 跳表(skiplist)

O(1) 获取元素，

跳表(skiplist) O(logN) 排序

ziplist,元素个数超出 128, skiplist

ziplist,单个元素大小超过64 B ,skiplist

排序方式，分值score，用户程序定义。

根据分值，根据顺序获取数据。

分值增加

并集、交集



### 持久化

Linux 父子进程

父进程的数据，子进程是否能看到？

子进程的修改不会影响到父进程；

父进程的修改也不会影响到子进程；


#### RDB

时点性

save - 阻塞，场景：关机维护

bgsave - fork创建子进程

配置文件

等OS后台进程写磁盘

设置 多少次更新操作或时间段 生成快照

后台进程生成快照，不影响主进程。

copyofwrite.  父进程修改a 数据，发现当前的a 只读，开启新的地址空间，a 指向新的内存空间

生成快照的过程

1.主进程的写操作，不在快照中执行，快照只能读。

2.后台进行只负责生成当前时间点的快照，把当前的快照写入磁盘。


提供数据的弱一致性。


弊端

- 只有一个快照，dump.rdb

- Linux fork 子进程后，父进程的内存页只读，但进程需要写操作时会触发中断，如果大量的写操作会影响性能。






#### AOF


everyseconbd - 每秒fsync一次磁盘

always - 每次更新，fsync一次磁盘

no - 让OS后台进程决定，OS默认30秒fsync一次磁盘



OS 后台刷内存，超5秒或pagecache超10%，具体参数可配置。

DIO ？ 可以跨过操作系统内核？


#### 混合模式

启动使用RDB文件，启动完AOF。

RDB + 增量AOF



### 集群  

#### 主从复制

3.0 之前 哨兵模式

访问 sentinel 集群，sentinel 集群处理分发读写


监控，主从切换，通知

master 负责写

slave 负责读

哨兵 如何选主？？？？



#### cluster

3.0 之后

Master0 - slave0,slave1...  

Master1 - slave0,slave1...

Master2 - slave0,slave1...

Master负责读写，slave负责备份


一致性哈希的思想

CRC16进行映射

无主模型

redis 集群中每台服务器都存了所有的映射关系。先按规则映射到某一台，如果没有命中再映射返回给具体的服务器。

数据分治的问题，聚合问题难以实现。 - 事务问题，交集

hash tag




### 有序与无序

注意同一客户端发来的请求，是有序的，TCP有序的前提下。


亲密度处理。同一客户端、同一商品、同一业务发到同一服务器处理。 做有序




数据倾斜

1.某个key的访问量过大

2.


### 缓存常见问题

#### 缓存击穿

单个key

业务请求的数据，缓存没有，但DB有。每次都请求DB



#### 缓存穿透

业务请求的数据，缓存与DB都没有

布隆过滤器

布隆过滤器的实现

- 代码与bits数据都在客户端

- 代码在客户端，bits在redis

- redis集成布隆过滤器模块

布隆过滤器缺点

- bits位只能更多，不可能减少

如何支持数据的删除？？？

- 布谷鸟过滤器(Cuckcoo Filter)

- redis置null值，减轻DB压力

#### 缓存雪崩

大量的key失效，直接访问DB，对DB造成压力

大量请求的到来，命中率低。

一致性(双写)

均分设置随机过期时间


RDB 如何快速恢复 ？


redis 只存内存，内存满了怎么办？  分片，代理？


强依赖击穿方案

有时候12点一过，大量数据必须得无效。



### redis 数据同步






### fork()

Under Linux, fork() is implemented using copy-on-write

子进程 fork 父进程，相当于对父进程进行了快照。

父子进程访问共同的数据是访问同一地址，但如果父进程发生修改，父进程修改的数据会指向新的内存地址，子进程所对应的数据的内存地址仍然不变。



### AKF

单机 - 主备

单进程 - 

缓存

数据库


拆分业务 - 不同业务不同的redis

主备 如何保持一致性？

### redis 分布式锁


1.setnx

2.过期时间

3.守护线程，多线程


redission 

