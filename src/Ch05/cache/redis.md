# Redis
## 0x00 前言

本文为 Cheatsheet 类型文章，用于记录我在日常编程中经常使用的 MongoDB 相关命令。

 - Redis Shell
 - Redis 配套工具
 - Redis-Py
 - 常见问题
 - 踩坑记录

不定期更新。

<!-- more -->

## 0x01 Redis Shell

 - RedisClient
 - 通过网络或者 Dash 查看文档
 - Redis 官方自带工具

## 0x02 Redis 使用场景

1. 记录点赞数 hash
2. 记录最近帖子列表 便于快速显示 zset
3. 记录帖子的点赞人，和去重 zset
4. 相关内容。list
5. 计数器，用于分配 ID

### 分布式锁

基本用法就是

```
set lock:upgdatenewprofile true ex 5 nx
TODO: 搞事情
del lock:upgdatenewprofile
```

但是呢？这个逻辑还是有问题的，比如第一个线程的搞事情的时间大于 5s, 那么第二个线程就会加个锁，然后第一个线程释放掉锁。

于是第三个线程一看，哟，没锁，就开始搞事情。

这种情况可以使用可重入锁.（但可重入锁本身就会增加代码的复杂度）

### 延时队列

```
brpop
blpop
```

blocking 本身也会爆异常，所以，也要处理好异常。

## 0x03 Redis-Py

### 基本类型与其操作

- string
- hash
- set
- zset 有序集合

```
set/get
mget name1,name2,name3
mset name1 value1 name2 value2
expire name1 5
setex name1 5 value1
```

```
# 队列
rpush queue item1 item2 item3
llen queue
lpop queue
```

```
# 栈
rpush stack item1 item2 item3
llen stack
rpop stack
```

慢操作

```
lindex # O(n)

lrange queue 0 -1
```

```
# hash 操作
hgetall
hlen
hget
hset
hmset
```

```
# 有序列表
zadd
xrange
zrevrange
zcard
```

### 高级类型与其操作

## 0x04 常见问题

```
bgsave 镜像全量持久化 耗时长
bgsave 子进程创建之后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开.fork / cow
aof 增量持久化，定期 aof 重写，redis4.0 混合了 bgsave 和 aof, 效果更好
aof 如果每条数据 sync 一下，那么就会不丢数据。然而，鬼才会这么做。
```

## 0x05 踩坑记录

### 1. 无法磁盘持久化

用 scrapy 配合 scrapy-redis 抓取网页并且存储到 MongoDB 里面。

由于 scrapy-redis 重写了 scrapy 的几个核心模块，借助 redis 来实现多个 scrapy 节点从而实现分布式。

默认的 scrapy 设置会把 items 放在 redis 从而方便程序对 items 进行后续处理。这个设计很完美，只是美中不足的是，我常常需要抓取大量页面直接缓存到数据库中。这就导致了 redis 很快就满了。

于是很容易报出这么一个错误。

> (error) MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error.

出错原因如同提示所言，无法磁盘持久化。

基本上问题可能就是：
1. 磁盘满了。
2. redis 本身在某个地方配置了磁盘缓存的大小。
3. 其他权限之类的问题。

最快的解决方式就是删除占用磁盘的部分。

```bash
# 进入 redis-cli 删除 items
config set stop-writes-on-bgsave-error no
del xxx_html:items
config set stop-writes-on-bgsave-error yes
# 到 bash 下面检查磁盘，我的机器瞬间释放了 3GB 的磁盘空间
df -hl
```
备注：del 一次即可，因为有程序正在运行，所以当 del 之后，原来阻塞的程序接着开始运行。 xxx_html:items 会不断出现新的值。

Scrapy 立马就开始工作了（无需重启）

但是这也不是没有弊端的，依据官方文档所言，只有你完全不 care 数据持久化的情况下才可以使用这种方式

最好的方式当然是让 bgsave 完全 work 了

```
# redis-cli
127.0.0.1:6379> CONFIG SET dir /data/tmp
OK
127.0.0.1:6379> CONFIG SET dbfilename temp.rdb
OK
127.0.0.1:6379> BGSAVE
Background saving started
127.0.0.1:6379>
```

---
ChangeLog:
 - **2016-12-11** 重修文字
 - **2018-08-28** 常用场景
