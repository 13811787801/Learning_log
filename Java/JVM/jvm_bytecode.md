# ByteCode

## JVM Data Types
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

## Stack-Based Architecture

**JVM stack**: for each thread, a stack is allocated where local variables, method arguments, and return values are stored. Here is an illustration showing stacks for 3 threads.

![](https://user-gold-cdn.xitu.io/2019/11/22/16e919a00a3ac729?w=169&h=220&f=png&s=1854)


**Heap**: memory shared by all threads and storing objects (class instances and arrays). Object deallocation is managed by a garbage collector.

![](https://user-gold-cdn.xitu.io/2019/11/22/16e919bf184e55a2?w=163&h=130&f=png&s=1496)

**Method area**: for each loaded class, it stores the code of methods and a table of symbols (e.g. references to fields or methods) and constants known as the constant pool.

![](https://user-gold-cdn.xitu.io/2019/11/22/16e91af6a6b43bd4?w=369&h=307&f=png&s=25750)


A JVM stack is composed of frames, each pushed onto the stack when a method is invoked and popped from the stack when the method completes (either by returning normally or by throwing an exception). Each frame further consists of:

1. An array of local variables, indexed from 0 to its length minus 1. The length is computed by the compiler. A local variable can hold a value of any type, except long and double values, which occupy two local variables.  
2. An operand stack used to store intermediate values that would act as operands for instructions, or to push arguments to method invocations.

![](https://user-gold-cdn.xitu.io/2019/11/22/16e91b16de499d5b?w=923&h=375&f=png&s=27032)


## Bytecode Expolored


## Refrences


[Introduction to Java ByteCode](https://dzone.com/articles/introduction-to-java-bytecode) 左耳朵耗子课程推荐


