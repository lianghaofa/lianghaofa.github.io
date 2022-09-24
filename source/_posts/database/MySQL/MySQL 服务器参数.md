---
title: MySQL 服务器参数
date: 2022-05-17 09:15:52
summary: MySQL 服务器参数
categories: 数据库
tags:
- MySQL
---
## MySQL 服务器参数优化


### character_set
MySQL utf8 不能存储所有的字符，遇到 4 字节的宽字符就会插入异常了， utf8mb4 可包括所有的字符。

纯英文，建议latin.

- utf8_unicode_ci和utf8_general_ci对中、英文来说没有实质的差别。

- utf8_general_ci校对速度快，但准确度稍差。

- utf8_unicode_ci准确度高，但校对速度稍慢。

如果你的应用有德语、法语或者俄语，请一定使用utf8_unicode_ci。一般用utf8_general_ci就够了。

### max_connections

指MySql的最大连接数，如果服务器的并发连接请求量比较大，建议调高此值，以增加并行连接数量，当然这建立在机器能支撑的情况下，因为如果连接数越多，介于MySql会为每个连接提供连接缓冲区，就会开销越多的内存，所以要适当调整该值，不能盲目提高设值。可以过'conn%'通配符查看当前状态的连接数量，以定夺该值的大小。

MySQL服务器允许的最大连接数 16384；

查看系统当前最大连接数：show variables like 'max_connections';

set global max_connections = 1024




### max_user_connections

是指每个数据库用户的最大连接。

针对某一个账号的所有客户端并行连接到MYSQL服务的最大并行连接数。简单说是指同一个账号能够同时连接到mysql服务的最大连接数。设置为0表示不限制。

目前默认值为：0不受限制。

这儿顺便介绍下Max_used_connections：它是指从这次mysql服务启动到现在，同一时刻并行连接数的最大值。它不是指当前的连接情况，而是一个比较值。如果在过去某一个时刻，MYSQL服务同时有1000个请求连接过来，而之后再也没有出现这么大的并发请求时，则Max_used_connections=1000.请注意与show variables 里的max_user_connections的区别。默认为0表示无限大。

查看max_user_connections值：show variables like 'max_user_connections';



### back_log

连接数 > max_connections + back_log, unauthenticated user

该值指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。也就是说，如果MySql的连接数据达到max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源。将会报：unauthenticated user | xxx.xxx.xxx.xxx | NULL | Connect | NULL | login | NULL 的待连接进程时.

back_log值不能超过TCP/IP连接的侦听队列的大小。若超过则无效，查看当前系统的TCP/IP连接的侦听队列的大小命令：cat /proc/sys/net/ipv4/tcp_max_syn_backlog目前系统为1024。对于Linux系统推荐设置为小于512的整数。


### wait-timeout

MYSQL 关闭一个非交互的连接之前需要等待的时间

MySQL默认的wait-timeout  值为8个小时，可以通过命令show variables like 'wait_timeout'查看结果值。

### interactive_timeout

非交互与交互的主要区别，是否需要长期持有资源。

服务器关闭交互式连接前等待活动的秒数。

交互式客户端定义为在mysql_real_connect()中使用CLIENT_INTERACTIVE选项的客户端。

## log 相关

### log

记录所有查询操作。

### log_error

记录错误信息

### log_update

开启update log。

### log_bin

开启 binary log, 主从复制。默认不开启。

### binlog_do_db

binlog_do_db = 指定mysql的binlog日志记录哪个db

binlog-ignore-db = 不需要复制的数据库苦命，如果复制多个数据库，重复设置这个选项即可


### sync_binlog

Redo log,

Undo log,

binlog

ACID

A - Undo log.

C - 通过 AID 来实现。 

I - 锁.

D - Redo log.

选择合适的异步持久化的时机。

用户态 ： Log Buffer   内核态 : OS Buffer




write 和 fsync 的时机是由 sync_binlog 参数来控制的。

1. sync_binlog = 0，表示每次提交事务write Log Buffer，每秒写OS Buffer，然后fsync 到磁盘。  (**操作系统后台会定时持久化**)

2. sync_binlog = 1, 表示每次提交事务都直接写OS Buffer， 然后 fsync 到磁盘.

3. sync_binlog = N, 表示每次提交事务都直接写OS Buffer， 累计N个事务后才fsync 到磁盘.

### log_slave_updates

如果使用链状同步或者多台Slave之间进行同步则需要开启此参数。






### basedir

MySQL主程序所在路径，即：--basedir参数的值。

### binlog_cache_size

为binary log指定在查询请求处理过程中SQL 查询语句使用的缓存大小。如果频繁应用于大量、复杂的SQL表达式处理，则应该加大该参数值以获得性能提升。

### bulk_insert_buffer_size

指定 MyISAM 类型数据表表使用特殊的树形结构的缓存。使用整块方式(bulk)能够加快插入操作( INSERT ... SELECT, INSERT ... VALUES (...), (...), ..., 和 LOAD DATA INFILE) 的速度和效率。该参数限制每个线程使用的树形结构缓存大小，如果设置为0则禁用该加速缓存功能。注意：该参数对应的缓存操作只能用户向非空数据表中执行插 入操作！默认值为 8MB。


### character_sets

MySQL所能提供支持的字符集。

### concurrent_inserts

如果开启该参数，MySQL则允许在执行 SELECT 操作的同时进行 INSERT 操作。如果要关闭该参数，可以在启动 mysqld 时加载 --safe 选项，或者使用 --skip-new 选项。默认为On。

### connect_timeout

指定MySQL服务等待应答一个连接报文的最大秒数，超出该时间，MySQL向客户端返回 bad handshake。

### datadir

指定数据库路径。即为 --datadir 选项的值。

### ft_min_word_len

指定被索引的关键词的最小长度。注意：在更改该参数值后，索引必须重建！

### ft_max_word_len

指定被索引的关键词的最大长度。注意：在更改该参数值后，索引必须重建！

### ft_max_word_len_for_sort

指定在使用REPAIR, CREATE INDEX, or ALTER TABLE等方法进行快速全文索引重建过程中所能使用的关键词的最大长度。超出该长度限制的关键词将使用低速方式进行插入。加大该参数的值，MySQL将 会建立更大的临时文件（这会减轻CPU负载，但效率将取决于磁盘I/O效率），并且在一个排序取内存放更少的键值。

### init_file

指定一个包含SQL查询语句的文件，该文件在MySQL启动时将被加载，文件中的SQL语句也会被执行。

### join_buffer_size

用于全部联合(join)的缓冲区大小(不是用索引的联结)。缓冲区对2个表间的每个全部联结分配一次缓冲区，当增加索引不可能时，增加该值可得到一个更快的全部联结。（通常得到快速联结的最佳方法是增加索引。）

### key_buffer_size

用于索引块的缓冲区大小，增加它可得到更好处理的索引(对所有读和多重写)。如果你使它太大，系统将开始变慢慢。必须为OS文件系统缓存留下一些空间。为了在写入多个行时得到更多的速度。

### language

用户输出报错信息的语言。

### large_file_support

开启大文件支持。



### long_query_time

如果一个查询所用时间超过该参数值，则该查询操作将被记录在Slow_queries中。

### lower_case_table_names

1: MySQL总使用小写字母进行SQL操作；

0: 关闭该功能。

注意：如果使用该参数，则应该在启用前将所有数据表转换为小写字母。

### max_allowed_packet

一个查询语句包的最大尺寸。消息缓冲区被初始化为net_buffer_length字节，但是可在需要时增加到max_allowed_packet个字节。该值太小则会在处理大包时产生错误。如果使用大的BLOB列，必须增加该值。

### net_buffer_length

通信缓冲区在查询期间被重置到该大小。通常不要改变该参数值，但是如果内存不足，可以将它设置为查询期望的大小。（即，客户发出的SQL语句期望的长度。如果语句超过这个长度，缓冲区自动地被扩大，直到max_allowed_packet个字节。）

### max_binlog_cache_size

指定binary log缓存的最大容量，如果设置的过小，则在执行复杂查询语句时MySQL会出错。

### max_binlog_size

指定binary log文件的最大容量，默认为1GB。

### max_heap_table_size

内存表所能使用的最大容量。

### max_sort_length

在排序BLOB或TEXT值时使用的字节数(每个值仅头max_sort_length个字节被使用；其余的被忽略)。






### thread_concurrency

该值的正确与否, 对mysql的性能影响很大, 在多个cpu(或多核)的情况下，错误设置了thread_concurrency的值, 会导致mysql不能充分利用多cpu(或多核), 出现同一时刻只能一个cpu(或核)在工作的情况。

thread_concurrency应设为CPU核数的2倍。比如有一个双核的CPU, 那thread_concurrency  的应该为4; 2个双核的cpu, thread_concurrency的值应为8。

比如：根据上面介绍我们目前系统的配置，可知道为4个CPU,每个CPU为8核，按照上面的计算规则，这儿应为：4*8*2=64

查看系统当前thread_concurrency默认配置命令：show variables like 'thread_concurrency';

### skip-name-resolve

禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。但需要注意，如果开启该选项，则所有远程主机连接授权都要使用IP地址方式，否则MySQL将无法正常处理连接请求！

### innodb_buffer_pool_size

主要针对InnoDB表性能影响最大的一个参数。功能与Key_buffer_size一样。InnoDB占用的内存，除innodb_buffer_pool_size用于存储页面缓存数据外，另外正常情况下还有大约8%的开销，主要用在每个缓存页帧的描述、adaptive hash等数据结构，如果不是安全关闭，启动时还要恢复的话，还要另开大约12%的内存用于恢复，两者相加就有差不多21%的开销。假设：12G的innodb_buffer_pool_size，最多的时候InnoDB就可能占用到14.5G的内存。若系统只有16G，而且只运行MySQL，且MySQL只用InnoDB，那么为MySQL开12G，是最大限度地利用内存了。

另外InnoDB和 MyISAM 存储引擎不同， MyISAM 的 key_buffer_size 只能缓存索引键，而 innodb_buffer_pool_size 却可以缓存数据块和索引键。适当的增加这个参数的大小，可以有效的减少 InnoDB 类型的表的磁盘 I/O 。

当我们操作一个 InnoDB 表的时候，返回的所有数据或者去数据过程中用到的任何一个索引块，都会在这个内存区域中走一遭。

可以通过 (Innodb_buffer_pool_read_requests – Innodb_buffer_pool_reads) / Innodb_buffer_pool_read_requests * 100% 计算缓存命中率，并根据命中率来调整 innodb_buffer_pool_size 参数大小进行优化。值可以用以下命令查得：

### show status like 'Innodb_buffer_pool_read%';

### innodb_log_buffer_size







