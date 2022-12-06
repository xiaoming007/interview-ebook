# finally面试题
在编译成字节码时，finally中的代码被复制了3份，分别放入try流程，catch流程以及catch剩余的异常类型，finally的代码一定会被执行。
### finally中出现return
```java
public static int test(){
	try{
		return 10;
	} finally {
		return 20;
	}

}
```
返回值为20
### finally 对返回值的影响
finally中不要进行return，如果在在finally中进行return，会出现异常被吞没的情况

