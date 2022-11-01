# String,==,equals
##### == 比较的内存地址
String对象，会存两份，一份放在常量池，一份放在堆内存中，通过一个映射表来映射到堆内的string
##### equals比较的是字符
```java
    public static boolean equals(byte[] value, byte[] other) {
        if (value.length == other.length) {
            for (int i = 0; i < value.length; i++) {
                if (value[i] != other[i]) {
                    return false;
                }
            }
            return true;
        }
        return false;
    }
```
##### String 类不能被继承
