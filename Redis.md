# Redis简介

## NoSQL

NoSQL指一种非关系型数据库，即Not Only SQL。

用于超大规模数据的储存。这些类型的数据储存不需要固定的模式，无需多余操作就可以横向扩展。

解决高并发，海量数据，高可用等问题。

## 背景

NoSQL一词最早出现在1998年，是Carlo Strozzi开发的一个轻量级、开源、不提SQl功能的关系型数据库。

2009年，NoSQL主要指非关系型、分布式、不提供ACID的数据库设计模式。同年，在亚特兰大举行的“no:sql(easy)”讨论会是一个里程碑，其口号“select fun profit from real_world where relational=false”。

## RDBMS vs NoSQL

**RDBMS**

- 高度组织化结构化数据
- 结构化查询语言SQL
- 数据和关系都储存在单独的表中
- 数据操纵语言，数据定义语言
- 严格的一致性
- 事务处理ACID

**NoSQL**

- 代表着不仅仅是SQl
- 没有声明性查询语言
- 没有预定义的模式
- 键值对储存Redis，列储存Hbase，文档储存MongoDB，图形储存Neo4J
- 最终一致性，而非ACID特性
- 非结构化和不可预知性的数据
- CPA定理
- 高性能，高可用性和伸缩性

## NoSQL数据库

### Memcache

- 很早出现的NoSQL数据库
- 数据都内存中，一般不支持持久层
- 支持简单的key-value模式，支持类型单一
- 一般是作为缓存数据库辅助持久化的数据库

### Redis

- 几乎覆盖Memcache的绝大部分功能
- 数据都在内存中，支持==持久化==，主要用作备份恢复
- 除了支持简单的key-value模式，还支持多种数据结构的储存，比如list,set,hash,zset等
- 一般是作为==缓存数据库==辅助持久化数据库

### MongoDB

- 高性能、开源、模式自由（schema free）的==文档型数据库==
- 数据都在内存中，如果内存不足，把不常用的数据保存到硬盘
- 虽然是key-value模式，但是对value（尤其是==json==）提供了丰富的查询功能
- 支持二进制数据及大型对象
- 可以根据数据的特点==替代RDBMS==，成为独立的数据结构，或者配合RDBMS，成为独立的数据库，或者RDBMS，储存特定的数据

# Redis安装

## 安装

需要gcc

```shell
yum install gcc
```

上传压缩包到`/opt`

```shell
rz -E
```

解压缩

```shell
tar -zxvf redis-6.2.6.tar.gz
```

编译

```shell
cd redis-6.2.5/
make
```

安装

```shell
make install
```

默认安装在`/usr/local/bin`

查看进程

```shell
ps -ef | grep redis
```

redis.conf

默认不开启进程守护

```
daemonize no
```

改成yes开启

## 相关知识

### 端口

端口6379（Merz一个女明星名字）

默认16个库初始从**第0号库**使用

统一密码管理，所有库的密码一样。

### 命令

使用命令`select <dbid>`切换数据库。例如，select 8

`dbsize`查看当前数据库的key的数量

`flushdb`清空当前库

`flushall`==通杀全部库==

### 单线程+多路IO复用技术

多路复用是指使用一个线程来检查多个我呢见描述符（Socket）的就绪状态，比如调用select和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个进程里执行，也可以启动线程执行（比如使用线程池）。

串行 vs 多线程+锁（memcached） vs 单线程+多路IO复用（Redis）

与memcached不同：支持持久化、支持多数据类型，单线程+多路IO复用

![image-20211108103425439](https://note-1010.oss-cn-beijing.aliyuncs.com/img/image-20211108103425439.png)

# 五大常用数据类型

## 类型

- redis字符串String
- redis列表List
- redis集合Set
- redis哈希Hash
- redis有序集合Zset

## 键

查看当前库下所有key

```shell
keys *
```

判断某个key是否存在

```shell
exists key 
```

查看key是什么类型

```shell
type key
```

删除指定key的数据

```shell
del key
```

根据value选择非阻塞删除(仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作)

```shell
unlink key
```

10秒钟：为给定的key设置过期时间

```shell
 expire key 10
```

查看还有多少秒过期，-1表示永不过期，-2表示已过期

```shell
ttl key
```

切换数据库(切换到1数据库)

```shell
select 1
```

查看当前数据库的key的数量

```shell
dbsize
```

清空当前库

```shell
flushdb
```

追加(对key追加abc)

```shell
append key abc
```



