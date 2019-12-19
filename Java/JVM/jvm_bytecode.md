# ByteCode

## Java虚拟机数据类型

The data types defined by the JVM are:

1. Primitive types:
    + Numeric types:
        1. byte (8-bit 2's complement)
        2. short (16-bit 2's complement)
        3. int (32-bit 2's complement)
        4. long (64-bit 2's complement)
        5. char (16-bit unsigned Unicode)
        6. float (32-bit IEEE 754 single precision FP)
        7. double (64-bit IEEE 754 double precision FP)
    + boolean type
    + returnAddress: pointer to instruction
2. Reference types:  
    + Class types  
    + Array types  
    + Interface types  

## Stack-Based Architecture / 基于堆栈的架构

**PC register / PC 寄存器**: for each thread running in a Java program, a PC register stores the address of the current instruction.
> 在 Java 程序上运行的线程，PC 寄存器存储指令的地址

**JVM stack**: for each thread, a stack is allocated where local variables, method arguments, and return values are stored. Here is an illustration showing stacks for 3 threads.
> 对每个线程，分配一个堆栈用来存储局部变量，方法参数，返回值。下面显示三个线程的堆栈

![JVM_stack](https://user-gold-cdn.xitu.io/2019/11/22/16e919a00a3ac729?w=169&h=220&f=png&s=1854)

**Heap**: memory shared by all threads and storing objects (class instances and arrays). Object deallocation is managed by a garbage collector.
> 所有线程内存共享(公共区),存储对象(类实例和数组),对象释放管理是通过垃圾回收

![Heap](https://user-gold-cdn.xitu.io/2019/11/22/16e919bf184e55a2?w=163&h=130&f=png&s=1496)

**Method area**: for each loaded class, it stores the code of methods and a table of symbols (e.g. references to fields or methods) and constants known as the constant pool.
> 对所有已加载的类，存储方法的代码和符号表(ps:说是对字段和方法的引用) 和被称为常量池的常量,下图展示了程序运行时常量池都能放了什么

![Method_area](https://user-gold-cdn.xitu.io/2019/11/22/16e91af6a6b43bd4?w=369&h=307&f=png&s=25750)

A JVM stack is composed of frames, each pushed onto the stack when a method is invoked and popped from the stack when the method completes (either by returning normally or by throwing an exception). Each frame further consists of:
> JVM 堆栈由帧组成，当一个方法被调用时，会推送帧到堆栈中，当方法执行完成，会从堆栈中推出(正常返回或是抛出异常)。帧包含了:

+ An array of local variables, indexed from 0 to its length minus 1. The length is computed by the compiler.
A local variable can hold a value of any type, except long and double values, which occupy two local variables.

> 一个局部数组，索引是从0到长度-1.这个长度通过计算编译(在编译时确定)  
> 局部变量可以存储任何类型的值，除了`long`和`double`，因为他们需要两个局部变量  
> array of local variables be called local variable table  
> array of local variables 保存方法的参数和局部变量值

+ An operand stack used to store intermediate values that would act as operands for instructions, or to push arguments to method invocations.

> 用于存储中间值的操作数堆栈，该中间值将用作指令的操作数，以及推送参数到方法调用

![img](https://user-gold-cdn.xitu.io/2019/11/22/16e91b16de499d5b?w=923&h=375&f=png&s=27032)

## Bytecode Expolored

每条指令都有遵循以下格式:  
`opcode (1 byte)      operand1 (optional)      operand2 (optional)`

1byte大小的操作码,包含要操作数据的操作数

### 案例1

```java
public static void main(String[] args) {
    int a = 1;
    int b = 2;
    int c = a + b;
}
```

通过 `javap` 工具查看字节码文件的操作,仅展示指令集

```bash
 Code:
      stack=2, locals=4, args_size=1
         0: iconst_1
         1: istore_1
         2: iconst_2
         3: istore_2
         4: iload_1
         5: iload_2
         6: iadd
         7: istore_3
         8: return
```

+ iconst: 加载int类型到 `operand stack`

+ istore: 存储int类型到 `jvm stack` 中的 `array of local variables`

+ iload: 从局部变量数组中加载值到 `operand Stack`

+ iadd: int类型的相加指令

### 案例2

代码

```java
public class CreatObject {

    public static void main(String[] args) {
        Point a = new Point(1, 1);
        Point b = new Point(5, 3);
        int c = a.area(b);
    }

}

class Point{
    int x, y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    public int area(Point b) {
        int length = Math.abs(b.y - this.y);
        int width = Math.abs(b.x - this.x);
        return length * width;
    }
}
```

字节码

```bash
public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=4, locals=4, args_size=1
         0: new           #7                  // class Point
         3: dup
         4: iconst_1
         5: iconst_1
         6: invokespecial #9                  // Method Point."<init>":(II)V
         9: astore_1
        10: new           #7                  // class Point
        13: dup
        14: iconst_5
        15: iconst_3
        16: invokespecial #9                  // Method Point."<init>":(II)V
        19: astore_2
        20: aload_1
        21: aload_2
        22: invokevirtual #12                 // Method Point.area:(LPoint;)I
        25: istore_3
        26: return
      LineNumberTable:
        line 4: 0
        line 5: 10
        line 6: 20
        line 7: 26
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      27     0  args   [Ljava/lang/String;
           10      17     1     a   LPoint;
           20       7     2     b   LPoint;
           26       1     3     c   I
}
```

+ new 创建对象-在堆中分配内存，在堆栈中传递引用(对象的引用被压入堆栈中)

+ dup 复制 `operand stack` 顶部的值,(一般跟在new命令后，复制对象2的引用)

> invokespecial 会消费一个对象的引用，所以需要复制保证堆栈中还能Point对象的引用

+ invokespecial 调用Point Class的构造方法，需要传输一个Point对象的引用，以及对应的构造器参数

+ astore_[slot] 存储对象的引用到stack的局部变量数组中(这也是为什么会用到dup指令的原因,invokespecial指令，会从operand stack中抛出对象的引用(消耗掉))

+ aload_[slot] 从局部变量数组中加载到 operand stack中

+ invokevirtual 这里是调用*area*方法,适配处理对象对应类型的方法(这涉及到重写，继承)

## Refrences

[Dzone:Introduction to Java ByteCode](https://dzone.com/articles/introduction-to-java-bytecode) 左耳朵耗子课程推荐

[IBM:Java bytecode](https://www.ibm.com/developerworks/library/it-haggar_bytecode/index.html)
