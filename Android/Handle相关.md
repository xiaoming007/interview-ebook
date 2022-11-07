# Handle相关
##### 1.所有的线程间通信框架原理
底层原理就是handler
#####  2.共享内存
##### 3.handler造成内存泄漏的原因
	引用链：static looper -> sMainLooper -> Messagequeue -> msg -> Handler -> MainActivity
##### 4.子线程如何创建handler
单开一个线程去创建looper,拿到looper后即可去在子线程创建handler，就是HandlerThread的原理
##### 5.子线程里创建的Looper不用的时候要调用quit进行释放
##### 6.handler如何处理发送延迟消息
##### 7.obtain()方法里直接取message,不回频繁的创建message
享元模式，epoll
##### 8.ANR的机制，定时器
##### 9.handler没有消息处理是阻塞的还是非阻塞的？为什么不会有ANR产生
消息是阻塞的，并不是所有的阻塞都会造成ANR
