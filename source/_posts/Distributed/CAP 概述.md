---
title: CAP 概述
date: 2022-05-22 10:24:52
summary: CAP 概述
categories: 分布式
tags:
- CAP   

---
## CAP 概述

需要 Partition 时，如何平衡  Consistency 和 Availability.

Consistency


Availability

Partition


### 分布式锁




#### 两阶段提交

检查 - 执行提交




1. 预提交 - 先走一遍流程

与第三方交互，不是直接跟DB提交。

中途出错就停止。减少了回滚的过程。

2. 执行提交

第三方是单机

- 可能宕机。

第三方是多机 - 事务不好处理

并发，需要等到反应。并发一大，效率底下。



操作日志文件

#### 

定时任务 

消息队列

固定时间

固定次数


#### 分布式事务






#### 三阶段提交




#### 自旋式分布式锁  - mysql, redis


redission







1. 开启后台线程，查看持有锁的线程是否还在。(防止某些线程突然被kill，无法释放锁)

2. 如果锁的线程还在，防止超时key无效，重新设置超时时间


key是否存在，不存在设置新的key并且设置超时时间，默认30S。

```java
<T> RFuture<T> tryLockInnerAsync(long waitTime, long leaseTime, TimeUnit unit, long threadId, RedisStrictCommand<T> command) {
        return this.evalWriteAsync(this.getRawName(), LongCodec.INSTANCE, command, "if (redis.call('exists', KEYS[1]) == 0) then redis.call('hincrby', KEYS[1], ARGV[2], 1); redis.call('pexpire', KEYS[1], ARGV[1]); return nil; end; if (redis.call('hexists', KEYS[1], ARGV[2]) == 1) then redis.call('hincrby', KEYS[1], ARGV[2], 1); redis.call('pexpire', KEYS[1], ARGV[1]); return nil; end; return redis.call('pttl', KEYS[1]);", Collections.singletonList(this.getRawName()), new Object[]{unit.toMillis(leaseTime), this.getLockName(threadId)});
    }
```


主从同步是异步进行的。

如果主节点与客户端确认了key,但没有来得及与从节点同步，主节点宕机了。


新选举出来的主节点没有key


Redis主从结构



AP 架构 - redis

如果主节点与客户端确认了key,直接向客户端返回成功信息，同时异步与从节点更新。

CP 架构 - zookeeper

主从一致后(接收到至少一台从节点确认信息)，才向客户端返回成功信息。


zookeeper - ZAB协议


RedLock - 多个同等级别的redis实例，需要超过一半redis节点加锁成功。


高并发分布式锁


多台机器的分布式锁，在单台机器内的多线程是否还需要上锁？

单台机器内的多线程上锁的好处？

1.减少对分布式锁的访问

分布式锁比单机锁慢，只是能提高更高的并发访问。


一个商品可以分成多把锁。


发布订阅，阻塞队列



redlock





####  event事件驱动 - zookeeper, etcd

分布式协调

分布式锁

分布式ID

分布式配置

分布式注册发现

分布式HA


Leader 单机，两阶段提交，过半通过

选举，退让制。互相发消息，数据ID，serverid

session 


watch 监控回调



### paxos 实现


集群内主机两两互相通信

sync



#### paxos 实现
zba - zookeeper


raft - journalnode




























