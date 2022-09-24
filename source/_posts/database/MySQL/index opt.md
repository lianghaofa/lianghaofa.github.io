---
title: MySQL 索引
date: 2022-05-09 07:08:52
summary: MySQL 索引优化
categories: 数据库
tags:
- MySQL
---


## MySQL 索引优化

### Memory 引擎 Hash

### Innodb 引擎 B+ tree

数据与索引位于同一的文件


默认一次读取 16kb

B tree
所有节点存储索引与数据

B+ tree

非叶子节点不存储数据


非叶子节点只存储存储索引

B* tree

如果没有主键，会自动生成64位的rowId.




### MySam 引擎 B+ tree

数据与索引位于不同的文件

遍历完索引后，获得指针


### 聚簇索引 Innodb
不是单独的索引类型，而是一种数据存储方式，指的是数据行跟相邻的键值紧凑的存储在一起。

优点：数据访问快

缺点： B + 树分裂合并次数更多，IO次数更多。

伟华索引成本更高




### 非聚簇索引  MySam
数据文件跟索引文件分开存放。

查询索引文件得到指针，使用指针寻找数据。

### MySQL 如何处理多级索引

多个索引，只有主键索引才跟数据放在一起。

#### 主键索引

不可以有null值

#### 唯一索引

可以有null值，null值比较较慢

#### 普通索引

普通索引可以重复

#### 全文索引

#### 组合索引

#### 索引合并

#### 面试技术名词

##### 回表

普通索引才存在回表

主键、name,先查name B+ 树，获取主键再查询主键的B +树 获取记录。

##### 覆盖索引

如果没有主键，MySQL会自动生成64位的rowId.

emp 表 索引 name, id 主键，默认建索引

select * from emp where id = 1

主键、name,先查name B+ 树，获取主键再查询主键的B +树 获取记录。

select id from emp where id = 1

直接查询主键的B+ 树 获取记录 。


**没有回表过程，就叫覆盖索引**

如果只需要查询索引列

MySAM 在内存中只缓存索引，数据则依赖与操作系统，可能需要IO操作。

InnoDB 使用聚簇，覆盖索引特别有效。


##### 最左匹配  适用于组合索引

select * from emp where name = ? and age = ?

先查name B+ 树，获取主键再查询主键的B +树 获取记录。

select * from emp where age = ? 

索引失效

select * from emp where name = ?

联合索引.

索引是排好序的数据结构 ？


索引是需要持久化的。尽量选短的的进行持久化。



where 语句，顺序会被优化调整。



##### 索引下推



select t1.name,t2.name from t1 jion t2 t1.id = t2.id

谓词下推

方式1：

t1 10个列， t2 10个列 -> 20列，然后把 t1.name t2.name 取出来

方式2：

t1.id t1.name t2.id t2.name -> 4列， 然后把 t1.name t2.name 取出来


组合索引  name, age

select * from emp where name = ? and age = ?

旧版本:

select * from emp where name = ?  全部取出来

对数据进行过滤 age = ? 

新版本： 索引下推

name，age 同时进行判断。



### 索引匹配方式

#### 全值匹配

索引中的所有列都匹配
primary key : id

index : name,age,pos

因此索引为：id,name,age,pos

select * from staffs where name = 'July' and age = '23' and pos ='dev';

查询索引为：name,age,pos
####  匹配最左前缀
select * from staffs where name = 'July' and age = '23';

查询索引为：name,age

select * from staffs where name = 'July';
查询索引为：name

#### 匹配列前缀

select * from staffs where name like '%J';
索引仍生效

select * from staffs where name like '%J%'; 

索引失效,%不要在前
#### 匹配范围

select * from staffs where name > 'Jason';
索引仍生效

#### 匹配某一列的全部与某一列的一部分

select * from staffs where name = 'Jason' and age > 25;
查询索引为：name,age

select * from staffs where name = 'Jason' and pos > 25;

仅name 生效

select * from staffs where name = 'Jason' and pos = 25 and age = 25;

仅name, age 生效，  pos = 25 不符合索引条件

select * from staffs where name = 'Jason' and pos = 'dev' and age = 25;

name, age,pos 生效。where 语句会进行优化


#### 只访问索引的查询

select name,age,pos from staffs where name = 'July' and age = '23' and pos ='dev';
直接获取数据

select * from staffs where name = 'July' and age = '23' and pos ='dev';

访问完索引，还需要获取其他列的值


### 优化细节

#### 使用索引进行查询的时候尽量不要使用表达式，把计算放到业务层。
where id + 1 = 5; where id = 4

#### 尽量使用主键查询，而不是其他索引。
避免触发回表查询

#### 使用前缀索引，IO读取前缀进内存进行过滤，前提是前缀能过滤大量数据，否则效果不明显。
减少IO次数

#### 使用索引扫描排序 order 

order by 排序，是在内存排序。索引是有序的， order by 索引

#### union all,in,or 尽量使用in
union 本身包含了distinct ，去重
union all 
select * from staffs where id = 1  union all where id = 2; 分开了sql语句执行


select * from staffs where id in(1,2); oracle 限制了 1000

select * from staffs where id = 1  or id = 2;  在前面语句的基础上过滤

#### 范围列可以用索引


#### 强制类型转换会全文扫描

前提 ： phone 列对应的类型为 varchar

select * from staffs where phone = 1234567   不会触发索引。



select * from staffs where phone = '1234567'   触发索引

#### 更新频繁，数据区分度不高的字段不宜直接建索引    

维护索引成本高

dict 高于 80%
#### 创建索引的列，不要为null， null的处理逻辑比较复杂

#### 当进行表连接的时候，最好不要超过3张表，需要join的字段，数据类型必须一致



MySQL join内部实现机制 ： 笛卡尔积  
##### Simple Nested Loop
a0   b0
a1   b1
a2   b2
a0 b0,a0 b1,a0 b2
a1 b0,a1 b1,a1 b2
a2 b0,a2 b1,a2 b2

##### Index Nested Loop
b 表设置索引 
a0   b0
a1   b1
a2   b2

a0 b1,a1 b1,a2 b1

先在索引查找，然后再回表查询记录。


A join B ，MySQL 读取的顺序不一定， conast jion 可指定读取顺序。

小表 join 大表
##### Block Nested-Loop Join

如果有索引，会采取Index Nested Loop。如果没有，会采用Block Nested-Loop Join，把驱动表中所有与join相关的列先读取到内存。

然后与磁盘中的另外一张表进行比较，成功直接返回缓存中的数据。


join_buffer_size 默认256KB


select * from t1 join t2 on t1.id = t2.id and t1.name = '张三';

select * from t1 join t2 t1.id = t2.id where t1.name = '张三';

结论：
inner join  当使用内连接时，两种方式一样

left join 当使用左外连接时，会把左表的数据全部查出。通过右表进行过滤
right join 当使用右外连接时，会把右表的数据全部查出。通过左表进行过滤


#### limit 限制输出
没有limit，指针继续往下寻找。

#### 单表索引控制在5个字段之内

索引字段越多，读取的数据量越多，可能导致IO次数增加。

#### 组合索引在5个字段之内

#### SQL优化要根据数据情况


### 索引监控 
show status like 'Handler_read%';

Handler_read_first: 通过index获取数据的次数
Handler_read_key: 获取索引第一个条目的次数
Handler_read_last: 读取索引最后一个条目的次数
Handler_read_next: 通过索引读取下一条数据的次数
Handler_read_prev: 通过索引读取上一条数据的次数
Handler_read_rnd: 从固定位置读取数据的次数
Handler_read_rnd_next: 从数据节点读取下一条数据的次数



B+树 可以限制高度?


Innodb_page_size 16384 字节  16KB

bigint 8B , 磁盘索引 6B

16KB / (8B + 6B) = 1170.285...


提前把非叶子节点(索引) 加载到内存，最多只需要一次磁盘IO即可获取数据。


MYSAM

.frm - 表结构

.MYD - data

.MYI - index

InnoDB

- 
.frm - 表结构

.idb - data 和 index

MYSAM - InnoDB B+树的不同在数据与索引是否分离

MYSAM - 叶子不存放数据

InnoDB - 叶子存放数据

聚集 与 非聚集 ？ 数据与索引是否一起存放

InnoDB B+树 的具体实现。。。说法不一致


B+树 - 叶子是所有节点

![B+树 叶子](/medias/MySQL/1656044298714.png)

MYSQL 叶子双向指针

![B 树 叶子](/medias/MySQL/1656044298714.png)

B 树 叶子没有指针

B 树 叶子没有冗余




![普通索引](/medias/MySQL/1656041969805.png)


InnoDB 没有主键，会在列种找一个唯一性的列作为索引，如果所有的列都没有唯一性，会维护一个主键。



### Buffer Pool (128M)


淘汰策略  LRU



空闲链表 free - 基节点(多少个空闲节点，头节点指针，尾节点指针) - 节点(指向空闲页的位置)  




持久化链表 flush -  修改后直接持久化，性能较差。放到 flush 链表

- 基节点(多少个持久化节点，头节点指针，尾节点指针) - 节点(指向持久化页的位置)


数据链表 LRU 

select * from table1, 当查询的数据量非常大，此时可能造成lru链表都被替换了。导致热数据也没了，影响性能。



分冷热数据，热数据不轻易替换。 5/8 热数据 , 3/8 冷数据。



冷数据 - 新数据块在冷数据间替换。

热数据 - 超过时间段的查询某个块的次数，以块为单位替换。  select * from table1 等操作会导致短时间内对某个块进行多次操作。


磁盘写数据，写一块？



update

1.修改Buffer Pool 相关的页数据

2.生成 bin.log

3.生成 redo.log

4.生成 undo.log

5.持久化 redo.log

6.后台定时任务负责持久化


update 语句执行完，不一定直接持久化，如果有事务，等事务提交完成。



事务的操作

1.操作位于内存的 Log Buffer

2.事务提交后才写入磁盘中的 redo.log 


MYSQL安装后占用两个  48M  的磁盘空间 用于  redo.log  ?。

参数可以调。innodb_log_file_size 为了达到顺序写磁盘的目的。

写入 redo.log ，并不代表真正完成持久化。仍需要写到数据真正所在的磁盘位置上。


- 0  事务提交时，不立即对 redo.log 进行持久化，交由后台线程操作。存在数据丢失的风险。

- 1  事务提交时，立即对 redo.log 进行持久化。

- 2  事务提交时，写入操作系统的缓冲区，并没有持久化。只要操作系统没挂，操作系统后台线程就会定时持久化，存在数据丢失的风险。


![Innodb-Architecture](/medias/MySQL/innodb-architecture.png)

Innodb -  redo.log


bin.log 属于数据库级别，不属于存储引擎级别。

bin.log - 主从同步

从库负责读，没有事务，此时可以把存储引擎设置为 MySAM

bin.log - 存储 sql

redo.log - 硬盘哪一页，哪一条数据。


redo.log 恢复数据比直接存储 sql 更快。



MVVC

DoubleWrite Buffer Files


InnoDB一个页 16KB, 操作系统一个页 4KB， 操作系统写一个InnoDB页需要4次，如果写了2次，操作系统挂了。该怎么办？

先写 双写缓存区。

InnoDB 的事务处理


MySQL 重启后需要恢复 redo.log ？


undo.log - 回滚数据。


a = 10

begin;
update a = 11;
end;

undo.log a = 10;


Change Buffer 


修改数据，可能会触发修改索引。不仅仅是数据

存放 uodate 语句，可能对索引进行了修改。等到需要查询索引时，再利用 Change Buffer 的数据同时更新记录。 懒更新


| 隔离级别      | 脏读 | 不可重复读   | 幻读   |
| :----: | :----: | :----: | :----:   |
| READ_UNCOMMITTED      | Y | Y   | Y   |
| READ_COMMITTED      | N | Y  | Y   |
| REPEATED_READ      | N | N   | Y   |
| SERIALIZABLE      | N | N   | N   |

Y 会出现相关问题， X 不会出现相关问题

SERIALIZABLE - 串行化

REPEATED_READ - 默认的隔离级别


REPEATED_READ - 会生成快照 ？

SERIALIZABLE - 会阻塞， 不管读还是写，如果有操作正在执行，都会阻塞。

READ_COMMITTED - 是否会阻塞 ？ 


### MVCC








