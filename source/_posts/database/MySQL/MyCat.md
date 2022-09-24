---
title: MyCat
date: 2022-05-14 10:15:52
summary: MyCat 分库分表
categories: 数据库
tags:
- MySQL
---
## MyCat

> 不同的请求，分发到不同的服务器。


### 如何判断请求类型， 分发到不同的服务器


高可用

负载均衡

SQL解析路由。抽象语法树

读请求还是写请求，分发到哪个库，哪个表。

数据散列，拆分数据。



### MyCat 处理流程 

解析，优化，路由，执行，数据整合返回




写操作，如何处理事务?

多张表，join ?

主从复制 binlog文件

延迟问题，如何处理。


读写分离

读写服务器的划分，多少台读服务器，多少写服务器。

分库分表

如何分库，如何分表，规则？

容灾备份

## 分布式数据库的实现原理

全局目录，分布式目录，全局与本地混合目录

### 数据分片


#### 水平切分-表切分
表切分
原来一张表存储在一台服务器
表的0-9999记录在服务器A
表的10000-19999记录在服务器B
表的20000-29999记录在服务器C ...



#### 垂直切分

原来一台服务器100张表
水平切分后，第一台30存30张表，第二台存30张，第三台存40张。

根据访问请求来分表，比如 jion 相关的表放在同一机器

### 分布式查询处理

sql查询语句拆分，到不同的服务器进行查询，查询完成后汇总。

### 分布式并发控制-事务

分布式事务处理。

### OLTP和OLAP

OLTP 联机事务处理  日常交易处理，面向实时交易类应用

OLAP 联级分析处理  统计、分析、报表，面向统计分析类应用


## MyCat 原理

拦截，分片分析，路由分析，读写分析分析，缓存分析，将SQL发送到后端所对应的数据库服务器，然后返回结果，最终返回给用户。

datanode,对应一台或多态数据库服务器。

分片字段 + 分片函数

ER分表，全局表等解决方法



### 应用场景

1.读写分离，主从切换

2.分库分表，垂直切分，水平切分

3.多租户应用，数据隔离。独立数据库；共享数据库，隔离数据架构；共享数据库，共享数据结构。

4.报表功能

5.整合多数据源

6.海量数据实时查询

7.数据库路由

### 为什么使用MyCat
java与数据库紧耦合，解耦。

高访问量高并发对数据库的压力，负载均衡。

读写请求数据不一致，读写分离，主从复制。

### MyCat高可用

HAProxy + keepalived

![MyCat高可用架构图](/medias/MySQL/1652494583.png)

### MyCat安全管理

#### 权限配置
##### user标签
| 标签属性      | 说明 |
| :----: | :----: |
| name      | 应用链接中间件逻辑库的用户名       |
| password   | 该用户对应的密码        |
| schemas     | 应用当前连接的逻辑库中所对应的逻辑表，schemas可以配多个       |
| readonly   | true只读，false读写，默认为false        |

```xml
<user name="mycat">
    <property name="password">123456</property>
    <property name="schemas">DB1</property>
    <property name="readOnly">true</property>
</user>
```
**重置后，记得重启**

##### privileges标签

| DEML权限      | 增加 | 更新   | 查询 | 删除  |
| :----: | :----: | :----: | :----: | :----: |
| 0001      | 禁止 | 禁止   | 禁止 | 允许  |
| 0010      | 禁止 | 禁止   | 允许 | 禁止  |
| 0100      | 禁止 | 允许   | 禁止 | 禁止  |
| 1000      | 允许 | 禁止   | 禁止 | 禁止  |

```xml
<user name="mycat">
    <property name="password">123456</property>
    <property name="schemas">DB1</property>
    <privileges check="true">
<schema name="DB1" dml="1111">
<table name="orders" dml="0000"></table>
</schema>
    </privileges>
</user>
```

#### SQL 拦截

##### 白名单
```xml
<firewall>
    <whitehost>
        <host host="192.168.85.11" user="mycat"></host>
    </whitehost>
</firewall>
```

##### 黑名单
| 配置项      | 默认值 | 功能   |
| :----: | :----: | :----: |
| selectAllow      | true | 是否允许执行select   |
| selectColumnAllow      | true | 是否允许执行select *  |
| selectIntoAllow      | true | 是否允许select 语句中包含 into 子句   |
| deleteAllow      | true | 是否允许执行delete   |
| updateAllow      | true | 是否允许执行update   |
| insertAllow      | true | 是否允许执行insert   |
| replaceAllow      | true | 是否允许执行replace   |

```xml
<firewall>
    <blacklis check="true">
        <property name="deleteAllow">false</property>
    </blacklist>
</firewall>
```


### 主从复制延迟问题 - 主从同步的问题 - 主从都负责读写

主从复制 ：master 向 slave 发送事件，  slave服务器 I/O thread 到主数据库拉取 binlog，写到 Relay log ,交由SQL thread 执行。

mysql的主从复制都是单线程的操作，
1.主库对所有DDL和DML产生的日志写进binlog，由于binlong是顺序写，所以效率很高

2.slave的sql thread将主库的DDL和DML操作事件在slave重放。

3.SQL执行，DDL和DML操作是随机（？？？指的是修改完的数据库文件位置不确定）的，不是顺序的，所以成本很高。

同时，由于sql thread是单线程的，当主库并发较高时，产生的DML数量超过slave的sql thread的处理速度，
或者当slave中右大型query语句产生了锁等待，那么就会产生延时。



1.读的访问大，备库更多，性能要求更高。

show slave status\G


![查看主从复制延迟](/medias/MySQL/1652500903.png)

> second behind master : slave 与 master 服务器 IO 线程之间的事件差距，单位以秒计

t1: 主库A执行完成一个事务，完成写入binlog的时刻
t2: 主库A传递给从库B，从库B把当前主库A的binlog复制完成的时刻
t3: 从库B完成此事务的时刻

t3 - t1 ： 从库B与主库A的延时


解决方案：
1.分库，平行扩展，分散压力。
2.读写分离，主写从读

读的访问大，备库更多，性能要求更高。

### 如何解决复制延时问题

#### 架构方面

1.分库架构，让不同的业务分散到不同的数据库服务器上，分散单台机器的压力

2.业务与mysql之间加入缓存，减少读压力，但对于数据经常发生修改，频繁更新缓存，需要考虑命中率的问题。由于命中率低，MySQL8.0之后，缓存没有了。

3.使用更好的硬件，cpr,ssd。

#### 从库配置方面

![文件写过程](/medias/MySQL/1652509663.png)

![binlog写过程](/medias/MySQL/1652510328.png)
开启事务 -> commit -> OS缓存 -> binlog缓存 -> 异步写入磁盘

每个线程都有自己的binlog cache，但是共用一份binlog。

图中的write,是把日志写到page cache,也就是 内存共用的 binlog cache，并没有把数据持久化到磁盘

图中的fsync，是把数据异步持久化到磁盘。  比较耗时

选择合适的异步持久化的时机。

write 和 fsync 的时机是由 sync_binlog 参数来控制的。

1. sync_binlog = 0，表示每次提交事务都只write，不fsync。  (**操作系统后台会定时持久化**)

2. sync_binlog = 1, 表示每次提交事务都fsync.

3. sync_binlog = N, 表示每次提交事务都只write，但累计N个事务后才fsync.

只位于内存，数据没有持久化，不安全。

如果出现主从复制延迟问题，sync_binlog 可以设置为100 - 1000某些值。 可能会丢失100 - 1000的数据。


2.禁止salve上的binlog. salve没必要记录。

3.innodb_flush_log_at_trx_commit 

非常强调安全性 -> 双1设置
innodb_flush_log_at_trx_commit.  sync_binlog


两阶段提交:保证数据安全

![查看bin相关参数](/medias/MySQL/1652509358.png)


![innodb数据更新流程](/medias/MySQL/innodb_update_process.jpg)


读远大于写的应用才适合主从复制。

#### MTS - enhanced multi-threaded slave - 并行复制

![coordinator](/medias/MySQL/1652513580.png)

![查看work](/medias/MySQL/1652513742.png)

![设置work](/medias/MySQL/1652513792.jpg)

![单个线程 ](/medias/MySQL/1652513921.png)

![多个线程 ](/medias/MySQL/1652514001.png)

为了保证主从数据的一致性。

coordinator 在进行分发的时候，需要遵循的策略:

1.不能造成更新覆盖，这就要求更新同一行的两个事务，必须分发到同一个worker中。 主键标识同一行，没有主键，后台会默认生成64位的唯一标识。

2.同一个事务不能被拆开，必须放到同一个worker中。- 同一个事务本来就是原子性操作不可再划分，不能拆分不同worker

按库分发，key -> 库名 ， val -> worker
按表分发，key -> 库名 + 表名 , val -> worker
按行分发，key -> 库名 + 表名 + 唯一键 ,  val -> worker


uodolog-回滚,redolog,binlog

redolog 组提交

binlog  组提交


5.6 只支持库

5.7 后支持行记录同步 -> 主键值标识

set global slave_parallel_type_= 'logical_check';


两阶段提交

组提交

MySQL5.7之后使用MTS并行复制，永久解决复制延时问题 ？？？？？

#### 组提交

##### redo log 组提交.  prepared commit

trx1 LSN:50,trx2 LSN:110,trx3 LSN:160

哪些事务可以归为一组。

trx1 第一个到达，选为leader

trx1要写hard disk. trx2 , trx3 也在到达了，一起写入磁盘。  LSN 小于等于160的都已经持久化到磁盘了。

##### bin log 组提交

 
mariaDB 的并行策略

Flush 阶段， Sync 阶段，Commit 阶段

等新事务的到来  -> Flush -> Sync -> Commit

等新事务的到来(1个或多个)  -> Flush Redo log(prepare), Write binlog -> Sync binlog-> Innodb Commit


每个阶段都有一个队列，每个队列都有一把锁，第一个进入队列的事务成为leader.


Flush 阶段

- 首先获取队列中的事务组

- 将Redo log 中prepare阶段的数据刷盘.

- 将bin log 数据写入文件，此时只是写入文件系统的缓存，如果数据库此时崩溃，binlog会丢失.

Sync 阶段

- binlog_group_commit_sync_delay = M; 在等待M us后，开始事务刷盘

- binlog_group_commit_sync_no_delay_count = N; 如果队列的事务在M us内达到了N个，直接刷盘

Sync 阶段队列的作用是支持binlog的组提交


Commit 阶段

- 首先获取队列的事务组
- 依次将redo log中已经prepare的事务在引擎层提交


#### MySQL5.7之后使用MTS并行策略

slave-parallel-type 控制并行复制的策略
- slave-parallel-type = DATABASE , 表示使用5.6版本的按库并行策略
- slave-parallel-type = LOGICAL_CLOCK ，表示使用 mariaDB并行策略 

MySQL5.7并行策略思想
- 同时处于prepare状态的事务，在备库执行是可以并行的
- 处于prepare状态的事务，与处于commit状态的事务之间，在备库上执行也是可以并行的。

基于这样的处理机制，我们可以将大部分的日志处于prepare状态，因此可以设置
- binlog_group_commit_sync_delay 参数，表示延迟多少微妙后才调用 fsync
- binlog_group_commit_sync_no_delay_count 参数，表示累积多少次以后才调用 fsync


#### 基于GTID主从复制问题

哪个文件，位置哪里。 

change master to master_host='192.168.86.11' .master_user='root' .master_password='123456' .master=3306.
master_log_file='master-bin.00001' .master_log_pos=154

GTID(global transaction) 是对于一个已提交事务的编号，是一个全局唯一的编号。 
GTID由UUID + TID组成，UUID是mysql实列的唯一标识，TID表示该实例上已经提交的事务数量。


##### 基于GTID的搭建


1.修改mysql配置文件，添加配置
- gtid_mode=on
- enforce-gtid-consistency=true

2.重启主从服务

3.从库执行如下命令
change master to master_host='192.168.86.11' .master_user='root' .master_password='123456' master_log_pos=1


无论是什么方式的主从复制其实原理相差都不是很大，关键点在于将组提交的信息存放在GTID中。

last_committed 值是上一个组事务的sequence_number值，last_committed 值相同表示在同一个组，可以并行执行。last_committed值是




















