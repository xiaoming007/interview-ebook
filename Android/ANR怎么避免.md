# ANR处理
##### 1.ANR场景
###### (1).Service Timeout
前台服务超时时间是20s，后台服务超时时间是200s
###### (2).BroadcastQueue Timeout
前台广播超时时间是10s，后台广播超时时间是60s
###### (3).ContentProvider Timeout
超时时间为10s
###### (4).InputDispatching Timeout
输入事件分发超时5s
##### 2.分析步骤
定位发生的时间点，查看生成的trace信息，分析问题，结合具体业务场景上下午分析
##### 3.如何避免ANR发生
主线程尽量只做UI相关的工作，避免耗时操作，eg：过度复杂的UI绘制，网络操作，文件io操作，避免主线程跟工作线程发生锁的竞争，减少系统耗时的binder的调用。
