# 内存管理

GC

Memory Management

- Manual Memory Management
- Auto Memory Management
 - stack allocation, region inference, memory ownership, and combinations of multiple techniques

Resources other than memory, such as network sockets, database handles, user interaction windows, file and device descriptors, are not typically handled by garbage collection. Methods used to manage such resources, particularly destructors, may suffice to manage memory as well, leaving no need for GC. Some GC systems allow such other resources to be associated with a region of memory that, when collected, causes the work of reclaiming these resources.

优点

Garbage collection frees the programmer from manually dealing with memory deallocation. As a result, certain categories of bugs are eliminated or substantially reduced:

- Dangling pointer bugs, which occur when a piece of memory is freed while there are still pointers to it, and one of those pointers is dereferenced. By then the memory may have been reassigned to another use, with unpredictable results.
- Double free bugs, which occur when the program tries to free a region of memory that has already been freed, and perhaps already been allocated again.
- Certain kinds of memory leaks, in which a program fails to free memory occupied by objects that have become unreachable, which can lead to memory exhaustion. (Garbage collection typically[who?] does not deal with the unbounded accumulation of data that is reachable, but that will actually not be used by the program.)
- Efficient implementations of persistent data structures

 "stop-the-world" stalls

 Strategies

 - Tracing
 - Reference Counting

## JVM

- G1
- Parallel
- Concurrent mark sweep collector (CMS)
- Serial
- C4 (Continuously Concurrent Compacting Collector) [25]
- Shenandoah
- ZGC

## 趣闻

- https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f
- https://www.geeksforgeeks.org/stack-vs-heap-memory-allocation/
