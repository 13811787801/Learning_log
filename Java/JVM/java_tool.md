# Java 命令工具

## javac

编译java文件

## java

执行.class文件

## javap

获取类和接口信息

+ javap -help 获取javap命令 的帮助消息

+ javap -v `className`

> -v or -verbose :
This option Prints additional information like stack size, number of locals and arguments for methods.  
此选项打印其他信息，例如堆栈大小，局部数和方法的参数。

## jmap

主要用于打印指定Java进程(或核心文件、远程调试服务器)的共享对象内存映射或堆内存细节

+ jmap -heap pid 根据进程号打印进行堆信息

+ jmap -histo:live `pid`
打印占用信息，实例数，类

+ jmap -dump:live,format=b,file=xxx.dump `pid`
根据进程号获取快照信息，使用MAT排查，优化

## jstack

打印Java线程的Java堆栈跟踪
