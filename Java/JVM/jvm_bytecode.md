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



## Refrences


[Introduction to Java ByteCode](https://dzone.com/articles/introduction-to-java-bytecode) 左耳朵耗子课程推荐
