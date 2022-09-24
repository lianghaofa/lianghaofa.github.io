---
title: MySQL 分区
date: 2022-05-14 20:45:52
summary: MySQL分区
categories: 数据库
tags:
- MySQL
---
## MySQL 分区

MySAM
.frm  表结构与元数据
.MYD  数据文件
.MYI  索引文件

InnoDB


静态分区

动态分区

分库分表
垂直 - 分表

水平 - 分记录

### 应用场景

#### 表过大，无法一次性装载到内存 

#### 热点数据，历史数据

#### 分区表数据更容易维护
- 更新，删除某个范围的数据

#### 避免某些瓶颈
- Innodb 单个索引的互斥访问
  
- exts文件系统的 Innodb 锁竞争

#### 备份和恢复独立分区

### 分区表的限制

#### 1024个分区，5.7后 8196 分区

Linux 打开文件的个数有限制 默认 1024 





### 分区表底层实现

- select

首先锁住所有的底层表，优化器过滤部分分区，然后再访问各个区的数据。

- insert

首先锁住所有的底层表，确定分区，然后写记录

- delete 
  
首先锁住所有的底层表，确定分区，然后删除

- update

首先锁住所有的底层表，确定分区，然后取数据，更新数据。


虽然每个操作都会打开锁住所有的底层表，但这并不是说分区表在处理过程中锁住全表，如果存储引擎能够自己实现行级锁(InnoDB),则会在分层释放对应表锁。


### 分区类型

#### 范围分区

```shell
    CREATE TABLE employees (
        id INT NOT NULL,
        fname VARCHAR(30),
        lname VARCHAR(30),
        hired DATE NOT NULL DEFAULT '1970-01-01',
        separated DATE NOT NULL DEFAULT '9999-12-31',
        job_code INT NOT NULL,
        store_id INT NOT NULL
    )
    PARTITION BY RANGE (store_id) (
        PARTITION p0 VALUES LESS THAN (6),
        PARTITION p1 VALUES LESS THAN (11),
        PARTITION p2 VALUES LESS THAN (16),
        PARTITION p3 VALUES LESS THAN (21)
    );
```

![分区文件](/medias/MySQL/1652673782.png)
插入 23 ，找不到合适的分区，报错。

```shell
    CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT NOT NULL,
    store_id INT NOT NULL
)
PARTITION BY RANGE (store_id) (
    PARTITION p0 VALUES LESS THAN (6),
    PARTITION p1 VALUES LESS THAN (11),
    PARTITION p2 VALUES LESS THAN (16),
    PARTITION p3 VALUES LESS THAN MAXVALUE
);
```

#### 列表分区

```shell
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY LIST(store_id) (
    PARTITION pNorth VALUES IN (3,5,6,9,17),
    PARTITION pEast VALUES IN (1,2,10,11,19,20),
    PARTITION pWest VALUES IN (4,12,13,14,18),
    PARTITION pCentral VALUES IN (7,8,15,16)
);
```

#### 列分区

类似 范围分区 与 列表分区

```shell
 CREATE TABLE rcx (
     a INT,
     b INT,
     c CHAR(3),
     d INT
 )
 PARTITION BY RANGE COLUMNS(a,d,c) (
     PARTITION p0 VALUES LESS THAN (5,10,'ggg'),
     PARTITION p1 VALUES LESS THAN (10,20,'mmm'),
     PARTITION p2 VALUES LESS THAN (15,30,'sss'),
     PARTITION p3 VALUES LESS THAN (MAXVALUE,MAXVALUE,MAXVALUE)
 );
```

```shell
CREATE TABLE customers_1 (
first_name VARCHAR(25),
last_name VARCHAR(25),
street_1 VARCHAR(30),
street_2 VARCHAR(30),
city VARCHAR(15),
renewal DATE
)
PARTITION BY LIST COLUMNS(city) (
PARTITION pRegion_1 VALUES IN('Oskarshamn', 'Högsby', 'Mönsterås'),
PARTITION pRegion_2 VALUES IN('Vimmerby', 'Hultsfred', 'Västervik'),
PARTITION pRegion_3 VALUES IN('Nässjö', 'Eksjö', 'Vetlanda'),
PARTITION pRegion_4 VALUES IN('Uppvidinge', 'Alvesta', 'Växjo')
);
```

#### HASH分区

```shell
CREATE TABLE employees (
id INT NOT NULL,
fname VARCHAR(30),
lname VARCHAR(30),
hired DATE NOT NULL DEFAULT '1970-01-01',
separated DATE NOT NULL DEFAULT '9999-12-31',
job_code INT,
store_id INT
)
PARTITION BY HASH(store_id)
PARTITIONS 4;
```

单纯取模，存储到对应分区。


#### KEY 分区


```shell
CREATE TABLE k1 (
    id INT NOT NULL PRIMARY KEY,
    name VARCHAR(20)
)
PARTITION BY KEY()
PARTITIONS 2;
```

#### 子分区

```shell
CREATE TABLE ts (id INT, purchased DATE)
    PARTITION BY RANGE( YEAR(purchased) )
    SUBPARTITION BY HASH( TO_DAYS(purchased) )
    SUBPARTITIONS 2 (
        PARTITION p0 VALUES LESS THAN (1990),
        PARTITION p1 VALUES LESS THAN (2000),
        PARTITION p2 VALUES LESS THAN MAXVALUE
    );
```
p0 继续分区
p1 继续分区
p2 继续分区

![子分区](/medias/MySQL/1652713964.png)






