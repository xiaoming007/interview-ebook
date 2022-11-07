# LeakCanary原理
##### 1.初始化流程
ContentProvider中初始化 -> 创建RefWatcher -> intall -> ActivityDestroyWatcher和FragmentDestroyWatcher(销毁观测)
##### 2.LeakCanary核心原理
开始检测 -> 创建KeyedWeakReference并与引用队列Queue关联 -> 把对象放入watchedReferences -> 工作线程中检测引用并GC -> 对象是否被回收（回收了就结束，没有回收就内存泄漏）
