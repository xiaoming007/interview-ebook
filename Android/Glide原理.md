# Glide原理
##### 1.Glide三个重要方法
###### (1).with
创建空白的Fragment管理生命周期机制,返回一个RequestManager
###### (2).load
最终构建RequestBuilder对象
###### (3).into
1.创建等待队列，执行队列 2.活动缓存 3.内存缓存 4. HttpUrlConnection
