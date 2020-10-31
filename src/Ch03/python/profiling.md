# Profiling/Tracing

- Profiling 定位与优化耗时、内存使用、CPU 使用
- Tracing 用于追踪内存布局

## 0x00 前言

本篇讨论的是优化

当我们在谈优化的的时候，首先要背诵下面三个口诀

	优化口诀 1: 先做对，布监控，再做好。
	优化口诀 2: 过早优化是万恶之源。
	优化口诀 3: 去优化那些需要优化的地方。

可以参考之前的文章 https://zhuanlan.zhihu.com/p/58754459

本文讨论的是基于现有代码的诊断。也顺带讨论了无侵入线上 trace 的原理和技巧

优化分为两种：

1. 侵入性诊断
2. 侵入性诊断

## 0x01 侵入性诊断

### 基础工具

- print
- logging
- timeit

### Profile vs cProfile

cProfile overhead 较高，

```python
import cProfile
import re
cProfile.run('re.compile("foo|bar")')

   197 function calls (192 primitive calls) in 0.002 seconds

Ordered by: standard name

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.000    0.000    0.001    0.001 <string>:1(<module>)
     1    0.000    0.000    0.001    0.001 re.py:212(compile)
     1    0.000    0.000    0.001    0.001 re.py:268(_compile)
     1    0.000    0.000    0.000    0.000 sre_compile.py:172(_compile_charset)
     1    0.000    0.000    0.000    0.000 sre_compile.py:201(_optimize_charset)
     4    0.000    0.000    0.000    0.000 sre_compile.py:25(_identityfunction)
   3/1    0.000    0.000    0.000    0.000 sre_compile.py:33(_compile)
```

### SpeedScope

- speedscope
- pyspeedscope

### PyInstrument

- https://github.com/joerick/pyinstrument
- https://github.com/joerick/pyinstrument_cext

### Line Profiler

https://github.com/pyutils/line_profiler

```bash
Pystone(1.1) time for 50000 passes = 2.48
This machine benchmarks at 20161.3 pystones/second
Wrote profile results to pystone.py.lprof
Timer unit: 1e-06 s

File: pystone.py
Function: Proc2 at line 149
Total time: 0.606656 s

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
   149                                           @profile
   150                                           def Proc2(IntParIO):
   151     50000        82003      1.6     13.5      IntLoc = IntParIO + 10
   152     50000        63162      1.3     10.4      while 1:
   153     50000        69065      1.4     11.4          if Char1Glob == 'A':
   154     50000        66354      1.3     10.9              IntLoc = IntLoc - 1
   155     50000        67263      1.3     11.1              IntParIO = IntLoc - IntGlob
   156     50000        65494      1.3     10.8              EnumLoc = Ident1
   157     50000        68001      1.4     11.2          if EnumLoc == Ident1:
   158     50000        63739      1.3     10.5              break
   159     50000        61575      1.2     10.1      return IntParIO
```

### 其他侵入性 Profiling 方案

- https://github.com/vmprof/vmprof-python
- https://github.com/bdarnell/plop

### Overhead 基数

Django template render × 4000

- Base 0.33s
- pyinstrument 0.43s 30%
- cProfile 0.61s 84%
- profile 6.79s 2057%

### 内存布局

- memory profiler 检查内存消耗
- pympler 快照未 GC 的对象

> TODO: python 有无侵入检查内存布局的工具么？

## 0x02 无侵入诊断

以上的代码都是侵入代码，Java 生态中存在着一些黑科技。可以在线打断点。

https://github.com/qunarcorp/bistoury/blob/master/docs/cn/debug.md

从而实现无侵入在线 tracing 代码，可以理解为一个无需 reload 代码即可实现的热插拔 logging

Python 世界里存在 2 个这样的工具

### pyflame

- https://github.com/uber-archive/pyflame

原理

- https://man7.org/linux/man-pages/man2/ptrace.2.html

### py-spy

- https://github.com/benfred/py-spy

直接读取 python 程序的内存信息

- Linux https://man7.org/linux/man-pages/man2/process_vm_readv.2.html
- macOS https://developer.apple.com/documentation/kernel/1585350-vm_read?language=objc
- Windows https://msdn.microsoft.com/en-us/library/windows/desktop/ms680553(v=vs.85).aspx

## 0x03 其他利器

### Django Debug Toolbar


