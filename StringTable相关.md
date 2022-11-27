# StringTable相关
他是运行时常量池的重要部分，特性
* 常量池中的字符串仅是符号，第一次用到时才会变为对象
* 利用串池的机制，来避免重复创建字符串对象
* 字符串变量拼接的原理是 StringBuilder（1.8）
* 字符串常量拼接的原理是编译期优化
* 可以使用intern()方法，方法的主要作用是将堆内存中的字符串对象拷贝一副本份到StringTable中，主动将串池中还没有的字符串对象放入串池(默认会直接将生成的对象，放入串池中)
### 常量池与串池的关系
常量池中的信息，都会被加载到运行时常量池中，这时a，b，ab等的字符串还只是常量池中的符号，还没有变为java字符串对象，当代码执行到字符串创建的时候，会先在StringTable中查看是否存在，没有就会把a变成“a”放进去。StringTable底层是hashTable结构，不能扩容
```java
public class Main {
    public static void main(String[] args) {
        String s1 = "a";
        String s2 ="b";
        String s3 = "ab";
        String s4 = s1 + s2; // s4是保存在堆内存中的，执行的是 new StringBuilder().append("a").append("b").toString().相当于new String()
        String s5 = "a" + "b"; // javac 在编译期间的优化，结果已经在编译期确定为ab了，不会变
        System.out.println(s3 == s4); // false
        System.out.println(s3 == s5);  // true

    }
}
```
### 字符串延迟加载
执行到某一句代码，如果StringTable中没有才会将其放进去
### 为什么将StringTable从方法区的永久代里，改为放在堆区
由于项目中会大量的使用字符对象，放在方法区里，当产生大量的字符串时，不会进行垃圾回收，但是如果放在堆区，如果有不再使用的字符串对象的时候当垃圾回收的时候就会对它进行回收，更好的优化内存空间。
### StringTable的垃圾回收
当创建过多的string对象的时候，超过了阈值就会触发GC
### StringTable性能调优
* 底层是一个hashTable可以通过调整桶的个数，来调整它的执行速度
* 考虑将字符串进行入池，调用intern()方法
