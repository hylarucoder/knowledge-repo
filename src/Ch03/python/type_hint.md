# Python Type Hint

## 0x00 前言

本文是《提升你的 Python 项目代码健壮性和性能》系列的第一篇文章。

本系列仅仅从笔者的项目经历来讲解一些提升代码健壮性的姿势和小技巧。

本文目录如下：

    ▼ 0x00 前言 : section
    ▼ 0x01 Gradual Typing : section
            静态类型 VS 动态类型 : section
            Gradual Typing = 静态类型 + 动态类型 : section
    ▼ 0x02 Python Typing 实战 - MyPY : section
            MyPy : section
            快速入门 : section
    ▼ 0x03 常见问题 : section
            如何忽略 mypy 警告 : section
            循环导入 : section
        0x04 Typing Anotation 项目最佳实践 : section
    ▼ 0xEE 参考 : section
            PEP : section
            扩展文章 : section

当我刚知道 Python 要添加类型的时候，我的内心是拒绝的。

但是，尝试了俩个疗程之后，腰也不疼了，腿也不疼了，走起路来都有劲了，嗯，真香。

为啥需要 Type Anotation?

因为软件开发需要协作，动态类型给人极大的灵活性，写的时候很爽，但如果解放了双手，撸起袖子一通写，自己写起来爽了，自己重构的时候或者其他人来看代码的时候，头发就会加速掉落。

加了 Typing 能解决这个问题嘛？不能，但适当的使用可以极大的提升代码的健壮性。

在如下的场景中，Typing 可以发挥作用

1. 在程序运行前进行类型检查
2. 提供 typing 信息，当然，这带来的另一个巨大的优点就是让 IDE 可以分析出函数的参数类型以及返回值

这样大大提升了代码量上来之后的类型检查不足带来的返工问题。

## 0x01 Gradual Typing

在你刚入门一门编程语言的时候，我们常常说，Java 是强类型（静态类型）语言，Python 是弱类型（动态类型）语言

从这两位诞生开始，静态类型和动态类型就一直进行旷日持久的圣战。

然而，而现在的发展趋势是：

- 静态类型的语言觉得自己太过静态，以至于写起来很啰嗦。于是引入了很多类型推断。 Java / Go
- 动态类型的语言觉得自己太过动态，以至于协作的过程中总是出现低级错误。于是引入了 Gradual Typing , Typescript / Flow / Python Type Annotation

什么是 Gradual Typing?

Gradual typing 允许开发者仅在程序的部分地区使用 Annotate/Type. 即，既不是黑猫（静态）, 也不是白猫（动态），从而诞生了熊猫（动静结合）。

话说回来，要知道为什么这么搞，首先要知道动态类型和静态类型会给程序带来什么。

### 静态类型 VS 动态类型

静态类型的语言，比如在写 Java 的时候，如果你把一个 int 赋值给了 string 的变量，IDE 会通过**类型检查器**立即报错并告诉你，你这个值赋值错啦。这个就是 Java 程序的检查阶段。 动态类型的语言，比如在写 Python 的时候，如果不用一些额外的手段，这种低级的错误，并不会在检查时爆出来，只会在运行时爆出来。如果线上还是出这个问题，就蛋疼了。

为了进行友好的讨论，本人将精分成 Javaer 和 Pythonist, 通过两人对话的方式，来讨论类型。

- Javaer: 我先喝杯咖啡
- Pythonist: 生命苦短，我用 Python。
- Javaer: P 哥，请（为什么叫 P 哥？Python 1989 年出生，Java 1995 年）
- Pythonist: J 弟，请
- Javaer: 静态类型可以较低成本的提早捕获 BUG, 比如：
    1. 你在写 Python 的时候，如果不用一些额外的手段，这种低级的错误，并不会在检查时爆出来，只会在运行时爆出来。
    2. 如果线上还是出这个问题，就蛋疼了。我这个类型检查可以在**使用 IDE 的时候给我分析出方法参数的类型和返回值**。所谓『上医治未病，中医治已病，下医治大病』, 防范于未然，善之善者也。
- Python: 等等，你小子还广征博引了还，首先，提早捕获 Bug, 我这里也有呀，比如我这里可以通过 flake8 来检查出有些没有定义的变量，**仅仅是类型没有检查而已**。其次，IDE 给我的补全又不是完全无法补全。弱一点罢了。你说的类型检查的问题：
    1. 可以通过**提升程序员的素质**来解决这个问题，或者让他们长点脑子，别特么在这种低级错误上犯错误。
    2. 写测试来**提升测试代码的代码覆盖率**（这个我会在本系列的第二篇文章里深入讲解）来解决这个问题
    3. 看看写的代码检查时出现问题，我完全可以**把代码拖到 IPython 里面跑一遍**。这可不仅仅能解决类型不正确带来的问题，还能快速解决代码的逻辑问题
- Java: 关于你说的第三点，我完全可以提升测试代码的覆盖率。哎？似乎我这个开发测试成本也上来了。看来**类型检查也不能解决这个问题**
- Javaer: 来 P 哥
    1. 静态类型确实以**较低的成本**解决了这种类型的问题，不是么？
    2. 并且，如果我其中一小块功能进行了修改，我总不能每次都跑 IPython 吧？我也不能因为想检查一下类型这种小操作就写测试代码覆盖一下？
- Python: 你每次修改，都要加类型，加类型，改类型，直到类型检查器完全接受。不麻烦嘛？面向类型检查器编程？
- Javaer: 来，
    1. 每次改代码的时候，又不是改一大推，你是小部分改的，能有多少项目是海量海量改？高内聚，低耦合，模块化开发。
    2. 好的代码是重构出来的，修改你的类型来让类型检查器通过。你的代码会被更好的组织起来。
    3. 我大 Java 就是面向重构的语言！我有 Jetbrain 的 IDE, 重构代码我怕谁
- Python: 来，你说的没错，
    1. **每次改代码的时候，又不是改一大推，你是小部分改的**。这话你说的没错，我也能用啊，因为代码总是一小部分一小部分改的，所以，改完了跑一下 IPython 就结了。
    2. 好的代码是重构出来的，修改你的类型来让类型检查器通过。你的代码会被更好的组织起来。这话你说的也没错，可**我重构的时候没有写测试就重构**，是不是有点莽撞？写了测试了，我还要花时间在类型检查器上，不啰嗦么？
    3. 我也有 Jetbrain 的 IDE, 重构代码我又不是不能重构。
- Python: 再来，
    1. 需求变更上来了，结果往往会出现，你本来是想专注于业务逻辑的更改的，但最后变成了大型**为了让类型检查器通过类型检查而艰苦奋斗的现场**, 我这个场景直接传 int/str/ 字典 / 传对象就很方便，你非要让我写四个函数来 override 方法。
    2. 虽然说，好代码确实可以通过重构出来，但动态语言表达能力强呀，你 Java HashMap 啰啰嗦嗦 put 写了半天，我 Python 一个 Dict 一把梭，看起来，清晰，改起来方便。

再比如说，

LeetCode 上面有一道题目，叫做最长连续 1

Input 是 [1,1,0,1,1,1] Output 是 3

我们尝试用 Python 来看下

    def find_max_consecutive_ones(num):return max(map(lambda x: len(x), ''.join([str(num) for num in nums]).split('0')))

我们尝试用 Java 来看下

    public class Solution {public int findMaxConsecutiveOnes(int[] nums) {int result = 0;int tmp = 0;for (int i = 0; i < nums.length; i++) {if (nums[i] == 0)tmp = 0;else {tmp += 1;result = Math.max(tmp, result);}}return result;}}

- Javaer: 啊咧？P 哥你确实有点短啊！
- Pythonist: 你敢说我短？你看看 java 的创始人的头发！

『贴图』

- Javaer: 我不是那个意思，浓缩就是精华嘛，表达能力弱又怎么样，我 Javaer 可以直接封装好这个功能当成工具类用，从外部使用上用起来差不多好吧，从项目角度表达力并不是决定性因素，静态类型检查可以提早在编译阶段做字节码优化。你的 GIL…
- Pythonist: 好了，咱就不要提 GIL 了
- Pythonist: 动态类型不需要花时间写 type annotation, 写起来速度杠杠的。
- Javaer: 静态语言一时爽，动态类型火葬场好伐？举个例子，太动态的东西，就是不好做类型推断，比如贵圈的著名的 sqlalchemy 做的那么动态，query.get() 结合 flask 来用，YouModel.query.get() 出来的 YouModel 你还要点进去查看一下具体属性，你要用 title 还是 name, 拼错了，怎么办？都不报错的。
- Javaer: 静态类型迫使你思考程序的时候更加严谨认真，这将会提升你的代码质量。
- Pythonist: 这点我是不服的，你只是花费了大量的时间在类型检查上，写的认不认真不完全取决于你编程的水平和态度好伐？假如你的观点成立，语言只是武器，峨眉师太拿一把倚天剑，不还是被张三丰空手取来？
- Javaer: 但你不能否认，峨眉师太拿着倚天剑确实可以秒杀很多人。

> 旁白君：有道是，梅须逊雪三分白，雪却输梅一段香。

- Guido van Rossum: 好了，我觉得类型不错，我在 dropbox 带领团队实现了 python 的 typing，python 3.7 内置哦。
- Pythonist: 我自己打脸一下，动态类型花点时间写 type annotation 代码健壮性杠杠的。
- Javaer: 你走开… 你怎么不去解决 GIL 的问题。

### Gradual Typing = 静态类型 + 动态类型

Gradual Typing 就是在动态语言的基础上，增加了可选的类型声明 (Type Annotation)

这对于我这种人是福音，

对于我个人而言，我是希望 Python 是有类型的

1. 作为某段程序的开发者和维护者，我可以提升我重构的速度。
2. 作为某段程序的调用方，可以快速的知道我调用后得到的东西究竟是什么。

但我又不希望这个声明不是强制性的

1. 我在构思程序的时候，想专注于接口的设计。在落实编码并且把代码写的足够的 dry 之后，在被调用的一些地方加上类型声明，这样可以提升我写代码的速度。

## 0x02 Python Typing 实战 - MyPY

### MyPy

mypy 是一个可选的静态分析器，官网介绍上说，mypy 将使你的程序更加易懂，调试和维护。

这个程序

- 对于 PHP 有 Hack , 对 JavaScript 有 Flow 和 TypeScript, 对于 Python 有 MyPy
- 对于 Python, 则有 MyPy , MyPy 彼时还不是很成熟 (2016 年 10 之前）。

Dropbox 的团队开发，Guido van Rossum 领导开发

### 快速入门

本小节部分摘录 Type hints cheat sheet

建议读者收藏原网址 https://mypy.readthedocs.io/en/latest/cheat_sheet_py3.html

    # 内置类型 x: int = 1x: float = 1.0x: bool = Truex: str = "test"x: bytes = b"test"child: boolif age < 18:child = Trueelse:child = False# 普通函数 def stringify(num: int) -> str:return str(num)# 生成器 def f(n: int) -> Iterable[int]:i = 0while i < n:yield ii += 1

直接看起来似乎，加不加 typing 对现在的代码改善并不是很明显嘛。

我们可以给复杂类型起别名：

    比如：
    def f() -> Union[List[Dict[Tuple[int, str], Set[int]]], Tuple[str, List[str]]]:
    def b() -> Union[List[Dict[Tuple[int, str], Set[int]]], Tuple[str, List[str]]]:

    AliasType = Union[List[Dict[Tuple[int, str], Set[int]]], Tuple[str, List[str]]]
    def f() -> AliasType:
        ...
    def b() -> AliasType:
        ...

看起来还行，但还是没有感觉到很明显的代码质量改善。

好，再看一例，使用 ClassVar 禁止属性无法在实例上设置

    from typing import ClassVar

    class A:
        x: ClassVar[int] = 0  # Class variable only

    A.x += 1  # OK

    a = A()
    a.x = 1  # Error: Cannot assign to class variable "x" via instance
    print(a.x)  # OK -- can be read through an instance

举个例子，flask-sqlalchemy, 可以通过 YouModel.query.get(id) 来拿到 YouModel 的实例，但 IDE 不能推断出这个实例是什么。

    # 方法一，Cast
    you_model_ins: YouModel = YouModel.query.get(id)
    # 方法二，包装一下 get 方法

    class YouModel(base):
        def get(id) -> "YouModel": # 注意这里的字符串
            pass
    you_model_ins = YouModel.get(id)

细心的读者可能看到这里的 YouModel 的返回值类型居然使用了 YouModel 的字符串，如果是 Java 的话，是可以直接写 YouModel 的。

    # 加上类型延迟求值
    from __future__ import annotations

    class YouModel(base):
        def get(id) -> YouModel:
            pass
    you_model_ins = YouModel.get(id)

还有其他的用法，请参考 MyPY 的官方文档

## 0x03 常见问题

### 如何忽略 mypy 警告

有的地方的代码不进行检查的话会方便很多。

与 flake8 类似，在注释后面写上标志就可以忽略了。

    youcode  # type: igonre

### 循环导入

我现在有两个文件，一个是 user.py 另一个是 order.py

在 user 里面有个方法需要返回 order 里面的 Order 列表，order 里面有个 order.owner 需要返回 User 实例。

如果不用类型声明的话，在 user 需要 order 的时候 import 进来即可规避循环导入。

在使用类型声明之后，建议在 user 里面这么写

    if TYPE_CHECKING:
        from project.models.order import Order # noqa

## 0x04 Typing Anotation 项目最佳实践

通过本文了解了基本的 Typing Anotation 的用法，其实效果还不够，本着对爱学习的读者老爷的负责的态度。

所谓『纸上得来终觉浅，绝知此事要宫刑』, 哦不『躬行』

推荐一个超级牛的大项目来让大家了解一下 typing annotation 的最佳实践。

https://github.com/zulip/zulip/

当然，从这个项目里面不仅仅能学到 typing annotation, 还能学到大项目下，牛 X 的公司的做法

1. 如何组织和划分模块
2. 如何帮助开发者快速启用开发环境。
3. 如何做测试，如何做 CI
4. 如何优化自己的 Workflow

有机会的话，我会挑其中的一小部分讲解一下。

## 0xEE 参考

### PEP

- PEP 3107
- PEP 483

### 扩展文章

- [关于 gradual typing](http://wphomes.soic.indiana.edu/jsiek/what-is-gradual-typing/)
- https://mypy.readthedocs.io/en/latest/cheat_sheet_py3.html
- https://blog.zulip.org/2016/10/13/static-types-in-python-oh-mypy/
- https://www.zhihu.com/question/21017354/answer/589574939

---

ChangeLog: - **2018-11-25** 初始化本文 - **2019-02-16** 重新整理文章
