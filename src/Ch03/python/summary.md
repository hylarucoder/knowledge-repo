# Python

## 0x01 前言

> Python 简略笔记

本文诞生于利用 Topic Reading 方法读 Python 和 JavaScript 若干本技术书籍这个过程中结合自己的开发常见场景记录下来的一些笔记。

## 0x00 简介

### 1. 为什么是 Python

选 Python, 很大程度上是因为 Python 的快速开发。

当然，快速开发（这里的开发包含部署）这个词也往往会被误解。什么叫做快速？我用一个 CMS 框架快速搭建出一个网站这是否叫做快速？

- 每一次部署的时候，如果使用 Java 或者是 Go, 部署的时候直接 maven 编译打包，接着把 War 包直接上传到 Tomcat 就结束了。而用 Python 则需要各种虚拟环境，各种稀里哗啦的配置。这种情况下是哪一种快速呢？

Python 有什么好处呢？

- 写代码效率高
- 生态圈好

写代码效率高，这指的是写 Python 代码，而不是运行时。

生态圈好，Web 开发用 Django/Flask , 数据抓取用 Requests , 数据分析清洗用 Pandas, 机器学习。

### 2. 工具链

### 4. 文档

### 5. 社区

### 6. 书籍

## 0x01 基本概念

> 程序 = 算法 + 数据结构

这句话当然是不全面的，但并不影响这句话在计算机世界里面的地位。

依我看来，对我的启发大致是：

> 我会把 API 的调用和数据结构以及算法想清楚，然后才动手把代码分解成伪代码。

### 1. 数据类型

数据类型按照不同的划分标准可以进行不同的划分：

按照复杂性可以这么划分：

- 简单类型
- 复杂类型

按照复杂性可以这么划分：

- 基本类型
- 引用类型

按照数据结构可以这么划分：

- 集合结构 : 串
- 线性结构 : 线性表 （单链表，静态链表，循环链表，双向链表，**栈，队列**)
- 树形结构 : 树（二叉树，B+ 树，红黑树）
- 图形结构 : 图

### 2. 操作

### 操作

对于一些基本的数据类型，操作为 加减乘除取余数位运算等等

对于复杂的一些数据类型，则需要对数据结构多一些了解。

比如，对队列而言，增删改查在算法复杂度上意味着什么？对机器的性能会不会有很多影响呢？ 比如，对 hash 而言，增删改查在算法复杂度上意味着什么？对机器的性能会不会有很多影响呢？ 比如，对字典而言，增删改查在算法复杂度上意味着什么？对机器的性能会不会有很多影响呢？ 比如，对字符串而言，增删改查在算法复杂度上意味着什么？对机器的性能会不会有很多影响呢？

那字符串来说，Java 推荐使用 StringBuilder 来合并多个字符串，Python 推荐 join 多个字符串等等。 #### 操作

### 3. 语句

## 0x02 中级概念

### 函数

### 作用域

### 模块

模块，这个概念，可大可小，大的时候，把一个程序说成是模块，小的时候，可以把一个文件，甚至你说这一个函数是一个模块，也行。

这里的模块指的是一个包下的函数。

### 面向对象

面向对象有三大概念：

- 封装
- 继承
- 多态

### 错误 / 调试测试

异常处理实际上可以考验一个程序员编写代码的健壮性。

事实上来说，代码写的健壮是一个程序员必备的素养。但其实在开发过程中，出于对项目进行赶工上线，需要对程序的健壮性做出一定的取舍。并且，在编写客户端，服务端，网页前端的时候基本上都会遇到这个问题。什么时候选择健壮的程序，什么时候选择是还可以的程序。需要自己的经验。

### IO 编程

### 进程和线程

### 多线程

> Python 多线程约等于并发。

### 多进程

### GIL

Global Interpreter Lock

并不是所有的解释器语言都有 GIL （尽管 Python 和 Ruby 里面都有）, 也并不是没有尝试过去除 GIL, 但是每次去除都会导致单线程性能的下降。所以暂时保留。

GIL 对程序中的影响：

> 一个线程运行 Python , 而其他 N 个睡眠或者等待 I/O - 同一时刻只有一个线程对共享资源进行存取 , Python 线程也可以等待 threading.Lock 或者线程模块中的其他同步对象；

### 协同式多任务处理

如果有两个线程，同时进行 IO 请求，当其中一个线程连接之后，立即会**主动让出 GIL**, 其他线程就可以运行。

> 当 N 个线程在网络 I/O 堵塞，或等待重新获取 GIL，而一个线程运行 Python。

让出之后还要执行代码呀，所以要有个收回 GIL 的动作。

### 抢占式多任务处理

Python 2 GIL , 尝试收回 GIL 为 执行 1000 字节码。 Python 3 GIL , 尝试收回 GIL 检测间隔为 15ms

### 线程安全

原子操作：sort 之类不需要 非原子操作：n=n+2 的字节码分为 加载 n , 加载 2 , 相加，存储 n, 四个步骤，由于不是原子性，很可能被由于 15 ms 而被打断。

当然，懒人一向是 : **优先级不决加括号，线程不决加 lock**

对于 Java, 程序员努力在尽可能短的时间内加锁存取共享数据，减轻线程的争夺，实现最大并行。但 Python 中，线程无法并行运行，细粒度的锁就没有了优势。

### 正则表达式

## 0x03 高级技巧

## 0x04 标准库

### 常用内建模块

### 系统化模块

1. Introduction
2. Built-in Functions
3. Built-in Constants
4. Built-in Types
5. Built-in Exceptions
6. Text Processing Services
7. Binary Data Services
8. Data Types
9. Numeric and Mathematical Modules
10. Functional Programming Modules
11. File and Directory Access
12. Data Persistence
13. Data Compression and Archiving
14. File Formats
15. Cryptographic Services
16. Generic Operating System Services
17. Concurrent Execution
18. Interprocess Communication and Networking
19. Internet Data Handling
20. Structured Markup Processing Tools
21. Internet Protocols and Support
22. Multimedia Services
23. Internationalization
24. Program Frameworks
25. Graphical User Interfaces with Tk
26. Development Tools
27. Debugging and Profiling
28. Software Packaging and Distribution
29. Python Runtime Services
30. Custom Python Interpreters
31. Importing Modules
32. Python Language Services
33. Miscellaneous Services
34. MS Windows Specific Services
35. Unix Specific Services
36. Superseded Modules
37. Undocumented Modules

## 0x05 第三方库

- Requests : API 人性化

## 0x06 代码质量

### 正确性

- 外部**不该**引用 protected member （单下划线）
- lambda 为一次使用，最好不要赋值。
- 不要给 buildin 函数赋值
- py3 直接 super()
- for in else 如果不内置 break 则出会在最后 for in 为 empty 的时候再执行 else 中的语句
- context exit 如果不 catch 掉异常让其自然向上一级抛出错误的话，必须为 (self, exception_type, exception_value, traceback):
- 不要在 init 里面 return 数据
- 不要混用 tab 和 space
- 4 个 space 缩进
- staticmethod 直接是 参数，classmethod 第一个参数为 cls
- 可变的 default value 是不能作为 参数的。（可能是解释器在确定函数的定义的时候完成赋值？)
- 遵循 exception hierachy https://docs.python.org/3/library/exceptions.html#exception-hierarchy
- defaultdict defaultdict(lambda : 6) , 必须 callable
- 尽量 unpack 赋值
- 字典用获取用 get(“myk”,None) , 赋值用 dictionary.setdefault(“list”, []).append(“list_item”)

### 可维护性

- 避免使用 import * , 我觉得这点值得商榷 , 如果是某个模块下，完全可以先把模块拆分成多个，最后 import 进来，接着使用 all.
- getxxx 获取实际值，如果不为实际值，返回 None 显然不如 try catch 来的实在。
- 避免使用 global
- 命名要注意
- 动态创建方法 , 我觉得这点值得商榷。

### 可读性

- 不要检查，如果可能有异常，尽量抛出异常来 trycatch 解决。
- a is None , if flag
- isinstance , not type(r) is types.ListType
- “{name}{city}”.format(**info_dict)
- for k , v in infodict.items()
- 使用 poiinfo = namedtuple(“poiinfo”,[“name”,“lng”,“lat”]) 返回 poiinfo[‘上海’,121.00,23] 最后返回值打印 poi.name , poi.lng , poi lat
- for numbers_value, letters_value in zip(numbers, letters):
- enumerate
- 如果能用 listcomp 则不使用 map 和 filter

### 安全性

### 性能

- 用 set
- d.iteritems() 比 items() 省内存

## 0xEE 文章更新

- **2017-05-11 19:43:00** : 增加代码质量模块
