# JVM Garbage Collection

## 垃圾回收的三种方式

当标记完所有的存活对象时，我们便可以进行死亡对象的回收工作了。主流的基础回收方式可分为三种

### Sweep / 清除

算法易实现 / 容易造成内存碎片

### Compact / 压缩

实现复杂，性能较低 / 没有内存碎片

### Copy / 复制

> 内存区域分为两等分，分别用两个指针 from 和 to 来维护，并且只是用 from 指针指向的内存区域来分配内存。当发生垃圾回收时，便把存活的对象复制到 to 指针指向的内存区域中，并且交换 from 指针和 to 指针的内容。复制这种回收方式同样能够解决内存碎片化的问题，但是它的缺点也极其明显，即堆空间的使用效率极其低下

算法易实现，解决了内存碎片问题 / 堆的使用率低下

## Stop The World

Full GC 会出现 Stop-the world 现象


## References

[coolshell:面向GC的JAVA编程](https://coolshell.cn/articles/11541.html)

[浅析JAVA虚拟机结构与机制](https://hesey.wang/2011/04/introduction-to-java-virtual-machine.html)
