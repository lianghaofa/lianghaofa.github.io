---
title: 消息中间件的作用
date: 2021-04-25 19:40:52
summary: 消息中间件的作用
categories: MQ
tags:
- ActiveMQ   

---
## MQ 功能


问题是，如何确实消息不丢失?

MQ 功能

### 存储消息(持久化)   

目的地(表)

Queue : 一次消费

Topic : 需要客户端订阅，订阅的客户端都能接收到消息。离线的不能接收。




### 通知


分布式思想

p2p 点多点
生产者 - 消费者  1 对 1

pub/sub
主题订阅， 1 对 多



### 应用场景

- 异步通信 
注册成功后，异步给用户发短信。
  
- 缓冲
限流，客户端无法处理的请求，先放到MQ
  
- 解耦
微服务之间的通信

- 冗余
解决数据一致性，存储数据，等待服务完成后，才删除信息。
  
- 异构系统的通信
例如，PHP服务 与 JAVA服务 之间的通信。

- 保证有序性


如何处理数据一致性问题。ACK确认？


![MQ对比](/medias/MQ/1652799703.png)

| 配置项      | 默认值 | 功能   |
| :----: | :----: | :----: |
| selectAllow      | true | 是否允许执行select   |
| selectColumnAllow      | true | 是否允许执行select *  |
| selectIntoAllow      | true | 是否允许select 语句中包含 into 子句   |
| deleteAllow      | true | 是否允许执行delete   |
| updateAllow      | true | 是否允许执行update   |
| insertAllow      | true | 是否允许执行insert   |
| replaceAllow      | true | 是否允许执行replace   |


zookeeper

ActiveMQ(一) done







