# Python Awesome

清单入选标准

1. 笔者正在使用的，即，过时的东西不入选
2. 并不拘泥于 Python, 即如果在其他语言里有更好的解决方案，我会在其他语言的 Awesome list 里推荐

## 0x01 环境搭建

- pyenv
- pyenv virtualenv
- poetry
- pipenv （更推荐 poetry)

## 0x02 爬虫

爬虫的流程如下，获取 - 解析 - 分析 - 入库

扩展的时候需要各自做好拆分

- 获取
- 解析
- 分析
- 入库

社区爱用 PySpider Scrapy 这类框架

也有喜欢直接手动组装的库来做爬虫的

分析

- charles 用于抓包和测试
- mitmproxy

请求

- requests
- headless-chrome
- socket

App 模拟点击

- appnium

解析

- json
- nodejs 配合 v8 引擎可以复用一部分 js 代码得出真实数据。
- beautifulsoup
- lxml
- pyquery

清洗与入库

并发

- multiprocessing
- threading
- asyncio
- gevent

## 0x03 Flask Web

```bash
flask-sqlalchemy
flask-migrate
```

## 0x04 Django Web

```bash
Django
celery
django-debug-toolbar
django-extensions
django-filter
django-grappelli
xadmin
django-mptt
django-redis
djangorestframework
```

## 0x04 FastApi Web

```bash
fastapi
uvicorn
jwt
psycopg2-binary
aiofiles
orjson
tortoise-orm
asyncpg
aioredis
aiokafka
```

## 0x07 调试与测试

```
ipython
jupyter notebook
ipdb
https://github.com/joerick/pyinstrument
https://github.com/joerick/pyinstrument_cext
speedscope
pyspeedscope
```

## 0x08 dry python

- https://github.com/dry-python/returns
- https://github.com/jd/tenacity

## 0x09 clear code

## 0x08 工具

## 0xBB News

- https://github.com/markshannon/faster-cpython

## 0xCC 推荐源码

### 简单

- kennethreitz/records: SQL for Humans™
- chrisallenlane/cheat
- jek/blinker: A fast Python in-process signal/event dispatching system.
- mitsuhiko/platter: A useful helper for wheel deployments.
- kennethreitz/tablib: Python Module for Tabular Datasets in XLS, CSV, JSON, YAML, &c.

### 中级

- faif/python-patterns 使用 Python 实现一些设计模式的例子。
- pallets/werkzeug flask 的 WSGI 工具集。其中包含了实现非常好的 LocalProxy,cached_property,import_string,find_modules,TypeConversionDict 等。
- msiemens/tinydb 了解用 Python 实现数据库。

### 高级

以及一个非常神奇的进阶项目 500lines https://github.com/aosabook/500lines

### 其他

- https://github.com/howie6879/ruia

## 0xDD 书籍

- [x] CPython Internals
- [x] Python Cookbook
- [x] Python Web 开发实战
- [x] Python For Data Analysis
- [x] 深入浅出 MySQL
- [x] 大型网站技术架构 - 核心原理与案例分析

## 0xEE 结语

---
ChangeLog:
- **2020-10-24** 重修文字
