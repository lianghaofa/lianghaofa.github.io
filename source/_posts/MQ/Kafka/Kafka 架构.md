---
title: Kafka 架构
date: 2022-05-20 19:40:52
summary: Kafka 架构
categories: MQ
tags:
- Kafka   

---
## Kafka 架构


### 数据同步机制

Kafka 的 Topic 被分成多个分区，分区是按照 Seqments 存储文件块。

0.11 版本之前 Kafka 使用 highwatermarker 机制保证数据的同步。

highwatermarker 的同步机制可能会导致数据的不一致或者乱序。

- LEO ： log end offset 每个分区中最后一条消息的下一个位置，即将写入的位置，分区的每个副本都有自己的 LEO。

- HW : high watermarker, HW之前的数据都理解是已经备份的数据，当所有节点都备份成功，Leader会更新水位线。

- ISR : In-sync-replicas,kafka的leader会维护一份处于同步的副本集和。replica.lag.time.max.ms 
  在此时间内，Leader没有到手Follower的fetch请求，踢出ISR列表。


![Kafka-highwatermarker](/medias/MQ/1652844163.png)

highwatermarker  的数据得到了备份。




#### 数据一致性

![Kafka-数据不一致](/medias/MQ/1653054011.png)

BrokerB 发现自己的水位线与Leader BrokerA 一致，没有替换数据。会导致数据不一致。


0.11 版本之后， Leader Epoch
![Kafka-Leader Epoch](/medias/MQ/1653100544.png)

对消息格式进行改进， 每个消息都携带一个4字节的 Leader Epoch 。

follower 变成 leader - 将 新的 leader epoch 当前 follower 的 LEO 添加到 leader_epoch_sequence 文件的末尾。
每个新来的消息都会携带这个新的 leader epoch 标记。

leader 变成 follower - 把本地最新的 leader epoch (2,1000) 给 新leader 发送epoch请求。
接收到新leader返回的 leader epoch(3,2000)

follower 的 leader_epoch_sequence 文件 与 主机的 leader_epoch_sequence 文件 可以不一致。

follower 发现自己的 leader epoch 的 offset 比 leader 小，直接获取新的数据。

follower 发现自己的 leader epoch 比 offset 比 leader 大，要截断本地的数据，与 leader 保持一致。

#### 数据丢失

![Kafka-数据丢失](/medias/MQ/1653053730.png)


BrokerB 会把消息截断，导致数据丢失。

数据丢失还是可能存在 ？？？？？

数据丢失是相当而言，可以配置 Acks 机制。数据都已经写入了所有的机器，此数据才能算是 Kafka 的一条记录。
当 Acks 的级别较低，数据丢失还是会存在，这要看业务的需要，设置不同的级别。但能够保证数据的一致性。

- acks = all， Leader将等待全部Follower的确认，只要有一个Follower能保存记录，记录就不会丢失。

引入 zookeeper ， zookeeper 存储所有分区的状态信息， zookeeper controller 负责监控各分区的状态信息。
一旦发现节点发生改变，controller 会把 epoch信息 发送给 leader，







