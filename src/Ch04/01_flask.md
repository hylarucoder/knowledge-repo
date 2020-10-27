# Flask

### 2.1 Templates

### 2.2 Testing Flask Applications

### 2.3 Application Errors

快速 Get 模板语言无非就是掌握：

1. 上下文变量
2. 条件语法
3. 列表语法
4. 模板的继承 (extend 语法）与组合 (include)
5. 额外的一些语法糖，比如 filter 的使用 / 组成

> Something that is untested is broken.

这里的测试，有哪些测试呢？

1. 非 flask 相关逻辑的测试。比如，我对一小段无关于 View 层的纯粹的逻辑进行测试，我比较喜欢使用 pytest 进行测试。
2. Flask 相关
- Setup
- TearDown
- 登录前 / 登陆后

比如测试请求与响应结果。比如测试路由。测试某个与 View 层绑定的数据操作是否执行成功。

首先，大致扫一眼 tutorial ，知道了 Flask 的教程讲了如下的东西：

1. 路由
2. 静态文件
3. 模板渲染
4. 接触请求数据
5. 重定向和错误
6. 响应
7. Session
8. Message Flash
9. 日志
10. 扩展

当然，我们都是老手了，肯定是挑重点来看了。

Routing, 发现现在的问题在于

Flask 有两个主要依赖： - 路由、调试、WSGI - 模板

## 0x02 社区支持

## 0x04 读文档产生的疑问

1.

For web applications it’s crucial to react to the data a client sends to the server. In Flask this information is provided by the global request object. If you have some experience with Python you might be wondering how that object can be global and how Flask manages to still be threadsafe. The answer is context locals:

Context Locals Insider Information If you want to understand how that works and how you can implement tests with context locals, read this section, otherwise just skip it.

Certain objects in Flask are global objects, but not of the usual kind. These objects are actually proxies to objects that are local to a specific context. What a mouthful. But that is actually quite easy to understand.

Imagine the context being the handling thread. A request comes in and the web server decides to spawn a new thread (or something else, the underlying object is capable of dealing with concurrency systems other than threads). When Flask starts its internal request handling it figures out that the current thread is the active context and binds the current application and the WSGI environments to that context (thread). It does that in an intelligent way so that one application can invoke another application without breaking.

So what does this mean to you? Basically you can completely ignore that this is the case unless you are doing something like unit testing. You will notice that code which depends on a request object will suddenly break because there is no request object. The solution is creating a request object yourself and binding it to the context. The easiest solution for unit testing is to use the test_request_context() context manager. In combination with the with statement it will bind a test request so that you can interact with it. Here is an example:

### Thread Local

One of the design decisions in Flask was that simple tasks should be simple; they should not take a lot of code and yet they should not limit you. Because of that, Flask has a few design choices that some people might find surprising or unorthodox. For example, Flask uses thread-local objects internally so that you don’t have to pass objects around from function to function within a request in order to stay threadsafe. This approach is convenient, but requires a valid request context for dependency injection or when attempting to reuse code which uses a value pegged to the request. The Flask project is honest about thread-locals, does not hide them, and calls out in the code and documentation where they are used.

[20180309Flask 源码解析](CheatSheet%20Flask/20180309Flask.md)
