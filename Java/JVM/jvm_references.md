# JVM

![jvm](https://hesey.wang/wp-content/uploads/2011/04/1353310.png)

[浅析JAVA虚拟机结构与机制](https://hesey.wang/2011/04/introduction-to-java-virtual-machine.html)

[JVM 解剖公园](https://shipilev.net/jvm/anatomy-quarks/)  
 是一个系列的文章，每篇文章都不长，但是都很精彩，带你一点一点地把 JVM 中的一些技术解开。

[JVM官方规格说明书](https://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf)

## TLAB (Thread Local Allocation Buffer)

这个技术是用于解决多线程竞争堆内存分配问题的，核心原理是对分配一些连续的内存空间

[浅析java中的TLAB](https://www.jianshu.com/p/8be816cbb5ed)
国内分析TLAB

[Introduction to Thread Local Allocation Buffers (TLAB)](https://dzone.com/articles/thread-local-allocation-buffers) 国外的blog

[JVM anatomy-quarks:TALB Allocation](https://shipilev.net/jvm/anatomy-quarks/4-tlab-allocation/)  
JVM解剖公园关于TLAB allocation的短文

## GC

[面向GC的JAVA编程](https://coolshell.cn/articles/11541.html)

## ByteCode

[Dzone:Introduction to Java ByteCode](https://dzone.com/articles/introduction-to-java-bytecode) 左耳朵耗子课程推荐,入门还不错
