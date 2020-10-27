# Celery

## 0x00 前言

本文编写于 2018 年初，于 2019 四月进行修订，也是笔者对 Celery 的系统梳理。

在我的文章如何保证 Django 项目的数据一致性中，提到了这么一个解决超卖的方案。

1. 在 Redis 里面直接生成 200 个订单号
2. 然后用户来一个取走一个订单号码
3. 通过 Celery 削峰 排队走异步任务
4. 最后通过数据表的 uniq 约束来防止下单超过 200 个。

https://zhuanlan.zhihu.com/p/57668068

有朋友和我讲，你这个方法是有问题的，走异步任务容易并发量太大，容易把数据库打爆。

其实是可以的，Celery 可以对 Worker 的 Task 限流 (ratelimit)。

## 0x01 Celery

### 为什么需要 Celery

在日常开发的时候，常常有一些『任务』需要处理。

1. 为了提升系统的响应速度，比如发送短信 / 发送邮箱，这类的『任务』可以走异步。
2. 为了在某个时间执行耗时操作，比如统计用户的文章 / 点赞 / 活跃度。
3. 为了削减峰值，比如秒杀系统的削峰走异步
4. 为了业务代码解耦，比如当我在知乎上更新文章，可能就会触发『推荐系统』,『文章管理系统』,『用户通知系统』

不用 Celery 的话，其实上面的业务也是能做的。 比如 1 中，可以直接启一个线程来做。比如 2 完全可以 Crontab 做一个定时任务。

那为什么要用 Celery 呢？

1. 把目光聚焦在 Task 的分发上面。而非线程，Deamon 之类细节的处理。
2. 方便，简单，易维护，高可用。
3. 便于监控。
4. 扩展性好。

基本上满足了你九成的需求。

## 0x02 Celery 快速开始

本文的讨论基于 Broker 为 RabbitMQ, Result Backend 为 Redis, Django 的 Web 应用，叫做 djoo

    sudo rabbitmqctl add_user djoo djoo
    sudo rabbitmqctl add_vhost djoo
    sudo rabbitmqctl set_user_tags djoo djoo
    sudo rabbitmqctl set_permissions -p djoo djoo ".*" ".*" ".*"

    CELERY_BROKER_URL = 'amqp://djoo:djoo@localhost:5672/djoo'

    CELERY_RESULT_BACKEND = "redis://{host}:{port}/1".format(
        host=os.getenv("REDIS_HOST", "localhost"), port=os.getenv("REDIS_PORT", "6379")
    )

泛读文档之后，需要搞清楚几个概念。

- Broker: 携带 Task 的消息中间件，是发送消息和接收消息的解决方案，比如 RabbitMQ
- Result Backend: Task 执行结果。
- Application: Celery 的实例
- Worker: 执行任务者
- Beat: 或叫做 Schedule, 一般用于执行定时任务。
- Task: 任务

## 0x03 Celery Guide

### Application

Application 可以针对整个 Celery 实例进行配置，比如配置时区，重写 Application 里的基类

### Tasks

Task 是一个 Class, 并且可以从任意 Callable 的对象创建。

Task Message 除非被 Acked, 否则不会从队列中移除。

> NOTE: 那什么时候算是 Acked?

    @app.task(name="xsum")
    def xsum(numbers):
        return sum(numbers)

- Tasks
- Calling Tasks
- Canvas: Designing Work-flows
- Workers Guide
- Daemonization
- Periodic Tasks
- Routing Tasks
- Monitoring and Management Guide
- Security
- Optimizing
- Debugging
- Concurrency
- Signals
- Testing with Celery
- Extensions and Bootsteps
- Configuration and defaults

### Application

### Tasks

### Calling Tasks

### Canvas: Designing Work-flows

### Workers Guide

### Daemonization

### Periodic Tasks

### Routing Tasks

### Monitoring and Management Guide

### Security

### Optimizing

### Debugging

### Concurrency

### Signals

### Testing with Celery

### Extensions and Bootsteps

### Configuration and defaults

## 0x03 RabbitMQ

发布者的消息经过交换机，分发到不同的队列，最后由接收方进行处理。

那么问题来了，交换机都是用来干嘛的

- Direct 单播路由：扔一条消息到一个队列中，依照 routingkey 投递
- Topic 多播路由：发给某几类队列（通知）.
- Fanout 广播路由：发给全部绑定在该路由上面的队列。
- Headers
1. 应用解耦。（平台无关，语言无关）

比如说，但项目足够大的时候，更新一个活动，可能需要更新用户的一些状态，可能要更新一波统计数据，可能要记录一批日志。这个时候原来的代码可能这么写：

    update_activity()update_user()update_user_cache()update_stats()record_user_log()

现在代码就可能这么写：

    send_task_update_activity()
    send_task_update_user()
    send_task_update_user_cache()
    send_task_update_stats()
    send_task_record_user_log()

1. 异步通信。（减轻请求峰值）

原本一个 webapp 不做异步的话，也能搞定，但做了异步之后，可以大幅度提升吞吐量和响应时间。

1. 数据持久化。（不丢失消息）
2. 送达保证。(ack late)

### 简单步骤

1. 定义 app, 指定 broker 和 backend
2. 定义 tasks
3. 指定 worker
4. 调用 Task , 调用后返回 AsyncResult 实例，

    add.delay(2, 2)add.apply_async((2, 2))add.apply_async((2, 2), queue='lopri', countdown=10)add.signature((2, 2), countdown=10)res = add.delay(2, 2)res.get(timeout=1)

### Groups

    # 并行
    group(add.s(i, i) for i in xrange(10))().get()
    # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
    # partial group
    g = group(add.s(i) for i in xrange(10))
    g(10).get()

### Chains

    chain(add.s(4, 4) | mul.s(8))().get()
    (add.s(4, 4) | mul.s(8))().get()
    # partial chain
    g = chain(add.s(4) | mul.s(8))
    g(4).get()

### Chords

    chord((add.s(i, i) for i in xrange(10)), xsum.s())().get()(group(add.s(i, i) for i in xrange(10)) | xsum.s())().get()# eg : upload_document.s(file) | group(apply_filter.s() for filter in filters)

## 0x07 踩坑集

- 序列问题

## 0xEE 参考链接

---

ChangeLog:
 - **2018-02-20** 初始化
 - **2019-04-04** 重修文字
