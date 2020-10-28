# Profiling

当我们在谈优化的时候，首先要背诵下面三个口诀

	优化口诀 1: 先做对，布监控，再做好。
	优化口诀 2: 过早优化是万恶之源。
	优化口诀 3: 去优化那些需要优化的地方。

可以参考之前的文章 https://zhuanlan.zhihu.com/p/58754459

## 0x01 侵入性优化

### 耗时

- timeit
- profile 与 cprofile
- line profile

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

- https://github.com/joerick/pyinstrument
- https://github.com/joerick/pyinstrument_cext
- speedscope
- pyspeedscope

### 内存

- memory profiler
- pympler

## 0x02 无侵入代码

- https://github.com/uber-archive/pyflame
- https://github.com/benfred/py-spy
- https://github.com/vmprof/vmprof-python
- https://man7.org/linux/man-pages/man2/ptrace.2.html

## 0x03 Web 项目优化

### Django Debug Toolbar

