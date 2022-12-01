# Handle相关
### 1.所有的线程间通信框架原理
底层原理就是handler
###  2.共享内存
### 3.handler造成内存泄漏的原因
	引用链：static looper -> sMainLooper -> Messagequeue -> msg -> Handler -> MainActivity
### 4.子线程如何创建handler
单开一个线程去创建looper,拿到looper后即可去在子线程创建handler，就是HandlerThread的原理
### 5.子线程里创建的Looper不用的时候要调用quit进行释放
### 6.handler如何处理发送延迟消息
MessageQueue -> next函数分拣消息的时候，会根据msg.when和当前时间计算出一个nextPollTimeoutMillis值，这个值就是控制消息延迟的。
nextPollTimeoutMillis决定了堵塞与否，以及堵塞的时间，三种情况:
等于0时，不堵塞，立即返回，Looper第一次处理消息，有一个消息处理完 ;
大于0时，最⻓堵塞等待时间，期间有新消息进来，可能会了立即返回(立即执行);
等于-1时，无消息时，会一直堵塞;
### 7.obtain()方法里直接取message,不回频繁的创建message
享元模式，epoll
### 8.ANR的机制，定时器
### 9.handler没有消息处理是阻塞的还是非阻塞的？为什么不会有ANR产生
消息是阻塞的，并不是所有的阻塞都会造成ANR
### Handler、Looper、MessageQueue、线程的关系
* 一个线程只会有一个Looper对象，所以线程和Looper是一一对应的 
* MessageQueue对象是在new Looper的时候创建的，所以Looper和MessageQueue是一一对应的
* Handler的作用只是将消息加到MessageQueue中，并后续取出消息后，根据消息的target字段分发给当初的那个handler，所以Handler对于Looper是可以多对一的，也就是多个Hanlder对象都可以用同一个线程、同一个Looper、同一个MessageQueue
### post(Runnable) 与 sendMessage 有什么区别
post(Runnable) 与 sendMessage的区别就在于后续消息的处理方式，是交给msg.callback还是 Handler.Callback或者Handler.handleMessage
### Handler如何保证MessageQueue并发访问安全的
循环加锁，配合阻塞唤醒机制,他的等待是在锁外的，当队列中没有消息的时候，他会先释放锁，再进行等待，直到被唤醒。这样就不会造成死锁问题了
### 什么是Handler的异步消息、同步屏障
线程的消息都是放到同一个MessageQueue里面，取消息的时候是互斥取消息（里面有大范围的synchronized代码块），而且只能从头部取消息，而添加消息是按照消息的执行的先后顺序进行的排序。如果有一个消息是需要立刻执行的，也是想要这个消息插队，那么就可以开启一个同步屏障，阻碍同步消息，只让异步消息通过，就是同步屏障的概念
### IdleHandler的使用场景
在 MessageQueue 类中有一个 static 的接口 IdleHanlder。当线程将要进入堵塞，以等待更多消息时，会回 调这个接口。简单点说：当MessageQueue中无可处理的Message时回调。作用：UI线程处理完所有View事务后，回调一些额外的操作，且不会堵塞主进程。常见的使用场景有：启动优化。
我们一般会把一些事件（比如界面view的绘制、赋值）放到onCreate方法或者onResume方法中。 但是这两个方法其实都是在界面绘制之前调用的，也就是说一定程度上这两个方法的耗时会影响到启动时间。
所以我们可以把一些操作放到IdleHandler中，也就是界面绘制完成之后才去调用，这样就能减少启动时间了。
### 为什么建议子线程不访问（更新）UI？
因为Android中的UI控件不是线程安全的，如果多线程访问UI控件那还不乱套了.
### Looper是干嘛呢？怎么获取当前线程的Looper？为什么不直接用Map存储线程和对象呢
通过ThreadLocal保存每个的线程的Looper，保证每个线程对应的自己的looper，不会混乱
### 可以多次创建Looper吗？
Looper的创建是通过Looper.prepare方法实现的，而在prepare方法中就判断了，当前线程是否存在Looper对象，如果有，就会直接抛出异常，所以同一个线程，只能创建一个Looper，多次创建会报错
### 利用Handler机制设计一个不崩溃的App？
主线程崩溃，其实都是发生在消息的处理内，包括生命周期、界面绘制。
所以如果我们能控制这个过程，并且在发生崩溃后重新开启消息循环，那么主线程就能继续运行

