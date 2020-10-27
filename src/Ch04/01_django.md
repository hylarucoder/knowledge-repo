# Django

## 0x00 前言

本文基于最新 Django 版本

1. 模型定义
2. Create/Update/Delete
3. 各种查询 / 链式调用 / F 表达式 / Window 函数 /Lazy Loading / Eager Loading
4. Join
5. DEBUG 和 Profile 技巧

本文是《Python ORM 三部曲的第一部 - Python 的三种数据源架构模式》

本文适用于：

1. 好奇 or 喜欢折腾的程序员
2. 想深入了解 ORM 的程序员

本文将解决你以下的疑惑：

1. 能不能不用 ORM?
2. ORM 为什么在某些场景下会胜于写 SQL
3. 不同的 ORM 实现机制会带来什么差异？

新工作的技术栈是以 Flask 为主，SQLAlchemy 是 许多玩 Flask 的人的标配。好，文档读起来，笔记搞起来。

> 所以，本文记录的是 Django ORM

逃。

这篇文章也是我对比 SqlAlchemy 以及 DjangoORM 的产物

## 0x01 如何快速上手

Django 世界里面，Django 的文档每次刷都会有新的发现。

## 0x02 DjangoORM 的基本功能

### 2.1 模型定义 Model

    from django.db import models

    class Musician(models.Model):
        first_name = models.CharField(max_length=50) # Field
        last_name = models.CharField(max_length=50)
        instrument = models.CharField(max_length=100)

    class Album(models.Model):
        artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
        name = models.CharField(max_length=100)
        release_date = models.DateField()
        num_stars = models.IntegerField()

        class Meta: # Model Meta
            order_with_respect_to = 'question'

可以看出，包含如下的部分：

1. Model 与 Model 内部的 Meta
2. Field 与 Field 内部的 Options
3. Model 与 Model 之间的关系
4. 其他，比如索引

### Models 与 Meta

> DjangoORM 是 ActivityRecord 模式的一种实现，在该模式下，Model 与 session 耦合。

### Field 与 Field Options

### Field

- AutoField
- BigAutoField
- BigIntegerField
- BinaryField
- BooleanField
- CharField
- DateField
- DateTimeField
- DecimalField
- DurationField
- EmailField
- FileField
- FileField and FieldFile
- FilePathField
- FloatField
- ImageField
- IntegerField
- GenericIPAddressField
- NullBooleanField
- PositiveIntegerField
- PositiveSmallIntegerField
- SlugField
- SmallIntegerField
- TextField
- TimeField
- URLField
- UUIDField

虽然有这么多东东，其实常用的如下

- BigIntegerField
- BooleanField
- CharField
- DateField
- DateTimeField
- DecimalField
- IntegerField
- TextField
- TimeField

有的字段属于那种，有也可以，没有也可以的。

FileField 之类的 往往现在都被 CDN 取代

### Field Options

- null
- blank
- choices
    - p.shirt_size
    - p.get_shirt_size_display()
- db_column
- db_index
- db_tablespace
- default
- editable
- error_messages
- help_text
- primary_key
- unique
- unique_for_date
- unique_for_month
- unique_for_year
- verbose_name
- validators

如此可见，Django 的 ORM 比起 SQLAlchemy 做了不少应用层的校验，一些 help_text

### Relationship

表和表之间的关系

1. A 表和 B 表 一对多 / 多对一
2. A 表和 B 表 一对一 （特殊的一对多）
3. A 表和 B 表 简单多对多 （借助中间的 Mapping 表进行映射）
4. A 表和 B 表 复杂多对多
5. A 表和 B 表 一对多

    from django.db import models

    class Manufacturer(models.Model):
        # ...
        pass

    class Car(models.Model):
        manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
        # ...

    from django.db import models

    class Topping(models.Model):
        # ...
        pass

    class Pizza(models.Model):
        # ...
        toppings = models.ManyToManyField(Topping)

    from django.db import models

    class Person(models.Model):
        name = models.CharField(max_length=128)

        def __str__(self):
            return self.name

    class Group(models.Model):
        name = models.CharField(max_length=128)
        members = models.ManyToManyField(Person, through='Membership')

        def __str__(self):
            return self.name

    class Membership(models.Model):
        person = models.ForeignKey(Person, on_delete=models.CASCADE)
        group = models.ForeignKey(Group, on_delete=models.CASCADE)
        date_joined = models.DateField()
        invite_reason = models.CharField(max_length=64)

    Models across files

    objects

    Models 重写方法

    Models 继承
    https://docs.djangoproject.com/en/2.0/topics/db/models/#model-inheritance

    组织代码 - 以及应对循环引用

### 2.2 QuerySet

### Create

    c = Child(name="苏轼")
    c.save()
    p = Parent(name="苏辙")
    p.best_child = c
    p.children.add([c,c2,c3,c4])
    p.save()

### Retrieve

过滤

filter(**kwargs) exclude(**kwargs) .all()

### 跨关系（跨表）查询

Blog.objects.filter(entry__headline__contains=‘Lennon’)

https://docs.djangoproject.com/en/2.0/topics/db/queries/#lookups-that-span-relationships

https://docs.djangoproject.com/en/2.0/topics/db/queries/#spanning-multi-valued-relationships

### prefetch_related && select_related

    select_related

    生成 join 的 SQL, 可以用来减少 N+1 , 不过仅仅支持一对多，和一对一

    prefetch_related

### Limit / Offset / 分页

Blog.objects.filter(entry__headline__contains=‘Lennon’)[30:20]

### 链式调用

query = query.filter(**kwargs) query = query.exclude(**kwargs)

### 表达式

Django 里面最强大的就是其 Q 表达式了

Q(question__startswith=‘Who’) | Q(question__startswith=‘What’)

Poll.objects.get( Q(question__startswith=‘Who’), Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)) )

这个表达式甚至可以嵌套超级深从而完成一个比较深的跨表查询。

> 并且，这种 API 查询反而在写前端 API 的时候，可以传入 question__startswith 这类参数，从而直接完成一组搜索。

Q / F

### 执行查询

需要注意的的是，SQLAlchemy 必须显式执行查询，而 Django 不一定。

https://docs.djangoproject.com/en/2.0/ref/models/querysets/#when-querysets-are-evaluated

在 Django 内部实现的时候，一个 queryset 创建 / 过滤 / 切片 / 传送，**除非这个 queryset 被 evaluated 了**, 否则不会做数据库的操作。

- iteration
- Slicing
- Pickling/Caching
- repr
- len
- list
- bool

    all()
    first()
    last()
    exist()

### 比较

- pk
- model

### 复制 实例

    blog = Blog(name='My blog', tagline='Blogging is easy')
    blog.save() # blog.pk == 1

    blog.pk = None
    blog.save() # blog.pk == 2

    # 但这个并不拷贝外键？???
    https://docs.djangoproject.com/en/2.0/topics/db/queries/#copying-model-instances

### 其他

    # 默认查询的是所有字段，但我希望查询部分字段
    # TODO: 阿萨德
    # Distinct
    # OrderBy

### 缓存机制

https://docs.djangoproject.com/en/2.0/topics/db/queries/#caching-and-querysets

### Update

单个 object 更新

    blog.title = "大宝天天见"
    blog.save()

批量更新

    query.update(headline=F('blog__name'))

一对多的更新（类似于 Set 操作）

    add(obj1, obj2, ...)
    create(**kwargs)
    remove(obj1, obj2, ...)
    clear()
    set(objs)

### Delete

### ForeignKey

    class Car(models.Model):
        manufacturer = models.ForeignKey(
            'production.Manufacturer', # 用来解决循环 circular import
            on_delete=models.CASCADE,
        )

on_delete 的情况

- PROTECT 阻止
- CASCADE 应用层的级联删除
- SET_NULL 应用层的级联 SET_NULL
- SET_DEFAULT
- DO_NOTHING
- SET() 一个 callback

### 聚集查询

https://docs.djangoproject.com/en/2.0/topics/db/aggregation/

    Book.objects.all().aggregate(Max('price'))
    Book.objects.aggregate(price_diff=Max('price', output_field=FloatField()) - Avg('price'))
    Book.objects.annotate(num_authors=Count('authors'))
    Book.objects.annotate(num_authors=Count('authors')).aggregate(Avg('num_authors')) # {'num_authors__avg': 1.66}

### Search

https://docs.djangoproject.com/en/2.0/topics/db/search/

### Window Function

    Option.objects.annotate(
            rank_num=Window(
                    expression=Rank(),
                    partition_by=F("vote_id"),
                    order_by=[F("current_vote_count").desc(), F("id").desc()],
            ),
            lag_vote_num=Window(
                    expression=Lag("current_vote_count"),
                    partition_by=F("vote_id"),
                    order_by=[F("current_vote_count").desc(), F("id").desc()],
            ),
    )
    .filter(vote=obj.vote)
    .order_by("-current_vote_count", "id")
    .values("id", "rank_num", "current_vote_count", "lag_vote_num")

### 2.2 Model Instances

https://docs.djangoproject.com/en/2.0/ref/models/instances/

### 2.3 模型实例

### 2.4 迁移机制

1. 加 INSTALLED_APPS

## 0x03 Django ORM 的高级功能

### maneger

https://docs.djangoproject.com/en/2.0/topics/db/managers/

### raw sql

https://docs.djangoproject.com/en/2.0/topics/db/sql/

### database-fuction

https://docs.djangoproject.com/en/2.0/ref/models/database-functions/

## 0x04 Database Access Optimization

### 4.1 使用连接池

连接池是一种永远在线模型的实现

连接池：驱动程序类型

连接池：代理类型

### 4.2 减少对 MySQL 的访问

    select count(*) from pg_stat_activity where pid <> pg_backend_pid() and usename = current_user;

### Before

    select count(*) from pg_stat_activity where pid <> pg_backend_pid() and usename = current_user;

### Profile First

1. django-extentions
2. django-debug-toolbar

手动

    queryset.explain

    from django.db import connection
    connection.queries

    from django.db import reset_queries
    reset_queries()

## 0x05 Django ORM Under The Hood

### 理解 QuerySet

- querysets-are-lazy when-querysets-are-evaluated
    - Iteration
    - Slicing
    - Pickling/Caching
    - len
    - repr
    - list
    - bool

    obj == nobj # obj.id == nobj.id

- caching-and-querysets

### 理解 cached attributes

    >>> entry = Entry.objects.get(id=1)
    >>> entry.blog   # Blog object is retrieved at this point
    >>> entry.blog   # cached version, no DB access

    >>> entry = Entry.objects.get(id=1)
    >>> entry.authors.all()   # query performed
    >>> entry.authors.all()   # query performed again

## 0xEE 参考链接

---

ChangeLog:
 - **2018-03-09** 重修文字
