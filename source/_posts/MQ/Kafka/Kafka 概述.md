---
title: Kafka 概述
date: 2021-04-25 19:40:52
summary: Kafka 概述
categories: MQ
tags:
- Kafka   

---
## MQ 功能


实时在线流处理

问题是，如何确实消息不丢失?

MQ 功能

### 存储消息(持久化)   



Queue : 一次消费

Topic : 需要客户端订阅，订阅的客户端都能接收到消息。


### 通知


分布式思想

p2p 点多点
生产者 - 消费者  1 对 1

pub/sub
主题订阅， 1 对 多



副本因子 partition

hash(key) % 分区数

副本因子 

每个分区存储的份数


分区leader  broker
broker - 某个分区的leader，其他分区的follower


![Kafka-Broker](/medias/MQ/1652840461.png)



某Borker 宕机，zookeeper 检测 ，一个Borker 可能是多个分区的leader

![Kafka-Broker](/medias/MQ/1652840461.png)

### 分区&日志

![Kafka-Broker](/medias/MQ/1652842212.png)

每个分区可以确保有序性，先进先出。

各个分区是独立的，无法保证分区间的有序性。





默认持久化168小时
log.retention.hours = 168


高并发 - 强调快速反应

大数据 - 大存储容量

消费者
![Kafka-消费者](/medias/MQ/1652844163.png)

每个消费者控制自己的偏移量，因此多个消费者之间彼此互相独立。

消费组

每个消费者实例都有Consumer Group,以Consumer Group为单位消费队列

均分原则，c5 不工作，等组内有实列停止， c5 会接替 c3

![Kafka-消费者](/medias/MQ/1652845519.png)




Kafka 持久化到硬盘，如何保证高速？ 

a properly designed disk structure can often be as fast as the network.

顺序写入  MMFile

- 并不是实时写入磁盘，利用操作系统分页存储，并不是主动刷磁盘，等待操作系统任务写磁盘

- 用户态写入内核态PageCache(是否可直接写)，由操作系统任务刷磁盘，断电掉数据。

![顺序写入  MMFile](/medias/MQ/1652847124.png)

服务器响应客户端读取请求。

ZeroCopy技术，磁盘直接到OS内存，然后把数据传输出去，不需要抵达用户空间。


![磁盘IO](/medias/MQ/1652847425.png)

![DMA](/medias/MQ/1652847663.png)

![常规IO](/medias/MQ/1652847724.png)

![ZeroCopy](/medias/MQ/1652847764.png)


sendfile system call.

the common way!
- 1.The operating system reads data from the disk into pagecache in kernel space
- 2.The application reads the data from kernel space into a user-space buffer
- 3.The application writes the data back into kernel space into a socket buffer
- 4.The operating system copies the data from the socket buffer to the NIC buffer where it is sent over the network

![the common way](/medias/MQ/1654091811.jpg)

- sendfile system call

allowing the OS to send the data from pagecache to the network directly.

![sendfile system call](/medias/MQ/1654091886.jpg)
