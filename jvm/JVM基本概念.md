# JVM基本概念
### JDK，JRE，JVM
JRE包含JVM和基础类库，JDK包含JVM和基础类库和编译工具
### JVM
他是java的虚拟机，便于跨平台开发而产生。学习了JVM可以便于深入理解实现原理
### 常见JVM
oracle的Hotspot也是我们在使用的
### JVM三大部分
java字节码，通过ClassLoader被加载到JVM中，类放在方法区，类创建的对象实例会被放在堆中堆里面的对象在调用具体的方法时，会调用虚拟机栈，程序计数器，本地方法栈。方法执行中，是由执行引擎中的解释器，解释执行，方法中的热点代码，即是被频繁调用的代码，会使用即时编译器，进行优化后执行，执行引擎中的GC会对堆中的不再使用的对象进行回收
#### 1.ClassLoader 类加载器
#### 2.JVM内存结构
* 方法区 Method Area
* 堆 Heap
* 虚拟机栈 JVM Stacks
* 程序计数器 PC Register
* 本地方法栈 Native Method Stacks
#### 3.执行引擎
* 解释器 Intercepter
* 即时编译器 JIT Compiler
* 垃圾回收器 GC
