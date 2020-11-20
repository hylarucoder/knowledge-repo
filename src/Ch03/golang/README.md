# Golang

## 模块 01：Go 语言实践 - Runtime

### Goroutine 的调度原理

- Goroutine 和线程的区别
- Goroutine 的实现、GPM 调度模型、调度状态及流转、调度原理、协作式抢占以及和网络库的协作

https://www.jianshu.com/p/105719434c29

https://zboya.github.io/post/go_scheduler/

https://studygolang.com/articles/16407

### 内存模型

Go 的内存分配机制
Go 内存分配的内部结构和分配机制

https://deepu.tech/memory-management-in-golang/

https://medium.com/a-journey-with-go/go-memory-management-and-allocation-a7396d430f44

### GC（垃圾回收）的原理

GC 三色标记算法

https://blog.golang.org/ismmkeynote

GC 介绍、三色标记的实现原理、GC 的流程以及 GC 的一些优化方案

### channel 的消息通讯原理

channel 的底层实现
channel 的通讯机制、环形队列的结构、调度和唤醒的原理

https://stackoverflow.com/questions/19621149/how-are-go-channels-implemented

## 模块 02：Go 语言实践 - 并行编程

### Memory Model

Go 中内存模型和同步语义

https://zhuanlan.zhihu.com/p/110032965
https://golang.org/ref/mem
https://golang.org/doc/

内存模型：Happens Before、同步语义、channel 通讯、锁

### 并发特性并发编程模式

https://blog.golang.org/pipelines

Timeout、Pipeline、Cancellation、Fanout、errgroup 等模式
利用 channel 完成并行开发的设计模式，包含超时控制、管道、扇出、errgroup 并发
无法熟练使用基于 channel 通讯方式的并行编程模式

### Context 的原理并掌握其使用方法

Context 传播式传递有什么意义
使用 Go 标准库 Context 的原理和最佳实践，包含超时控制、元数据传递、生命周期控制

https://blog.golang.org/context
https://levelup.gitconnected.com/context-in-golang-98908f042a57

## 模块 03：Go 语言实践 - 网络编程

### Golang TCP 网络编程

Go 实现高性能的 TCP Server

goim 项目了解 Go 语言中 TCP Server 的基础库和性能优化方案

### Golang HTTP 网络编程

HTTP 框架选型
针对业务需求对 HTTP 框架做针对性的扩展
结合 gin 项目了解 Go 语言中的 HTTP Server 的基础库和框架

## 模块 04：Go 语言实践 - 异常处理

### error 的处理方法
### 业务错误的处理方法

error 的处理复杂，不会正确使用
业务错误定义和 error 整合难度较高

Go 语言中的 error 处理实践：检查错误、定义错误、追加上下文
Go 项目中的业务错误码如何结合 error 的最佳实践

## 模块 05：Go 工程化实践

### 良好的项目目录组织原则和规范

### API 的设计方法和规范

### Package 的管理和设计方法

### 单元测试

1. Go 项目的标准化管理
2. 设计 API
3. 包管理

1. 良好的 Go 项目中的分层目录结构组织和代码规范
2. Go 项目中 API 的设计原则和方法：定义、状态和业务错误码处理
3. Go 项目中包的设计和最佳实践、go mod 的使用
4. go test 工具链的使用方法、单元测试的最佳实践以及 Mock 技术

## 模块 06：Go 架构实践 - 分布式架构（前端负载均衡）

### 高可用 DNS 的最佳实践

### CDN 的架构和应用场景

### 深入理解 4/7 层负载均衡的原理

在线服务的全链路视野
应用服务上层的负载均衡

DNS 的原理、防劫持的方法、HTTPDNS + IP 长连接
CDN 的系统架构、应用领域以及保证数据一致性的方法
LVS、Nginx 4/7 层负载均衡的原理和实践

## 模块 07：Go 架构实践 - 分布式架构（数据分片）

### 数据 Sharding 的设计原则
### 了解 Hash 分片的算法和演进历史

Sharding 的应用场景，不会合理使用
Hash 算法的使用场景

数据分片设计，如：分库分表、多活的 Shard 设计等等
分片算法的 Hash 实现和演进：Hash 求余、一致性 Hash、有界负载一致性 Hash、节点映射

## 模块 08：Go 架构实践 - 微服务（微服务概览与治理）

### 微服务的演进历史及优缺点
### 微服务的设计方法
### 微服务中 RPC 的底层原理

微服务的服务角色：API Gateway、BFF 还是 Service？
微服务拆分
RPC 的原理，不知道如何进行微服务 RPC 框架的选型

微服务的原理、概念，以及微服务的实现细节
API Gateway、BFF、Service 等概念精讲
微服务通讯 RPC 框架的细节和选型

## 模块 09：Go 架构实践 - 微服务（可用性设计）

### 可用性设计的最佳实践
### 可用性设计的几大关键点：隔离、超时控制、过载保护、限流、容错 & 重试

如何设计高可用的分布式服务
何提升服务自愈能力

微服务的隔离实现，以及架构设计中的隔离实现
进程内超时控制和跨进程超时控制
程序自保护避免过载，抛弃一定的流量完成自适应限流
单机限流、多租户场景的分布式限流
节点故障的容错逻辑、重试容错的策略和设计

## 模块 10：Go 架构实践 - 中间件（日志、指标、链路追踪）

### 日志收集
### 监控指标体系
### 分布式链路追踪

如何解决微服务的可观测性难题
怎么做微服务的可视化和标准化
出故障后，难以对微服务进行问题诊断

实现一个可以集中收集所有微服务实例的日志，并能统一查看和检索的日志采集架构
指标监控、使用 Prometheus 解决监控可视化、指标采集
微服务中的跨服务性能问题诊断，结合 Jaeger 实现分布式链路追踪

## 模块 11：Go 架构实践 - 中间件（缓存、数据库）

### Redis、Memcache 的原理和实战技巧
### MySQL 的常用设计和优化方法

1. 解决缓存的一致性问题
2. 怎样合理地设计 MySQL 的表

1. Redis、Memcache 的应用场景、最佳实践，以及缓存的一致性设计
2. MySQL 的表设计、常用优化手段，以及如何解决分布式事务

## 模块 12：Go 架构实践 - 中间件（消息队列、服务发现）

### 深入理解消息队列的原理，掌握基于消息队列的架构设计方法
### 服务发现原理、选型策略，以及服务发现实现的微服务多租户架构

1. 消息解耦的架构设计
2. 如何实现服务发现对平滑发布的支持
3. 怎样利用多租户实现多测试环境

1. Kafka 的实现原理、异步消息系统的架构设计
2. RPC 服务发现、动态地址的选型和实现原理，以及基于服务发现的平滑重启和多租户架构


