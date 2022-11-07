# retrofit原理
##### 1.Okhttp使用中的缺陷
###### (1).用户网络请求的接口配置繁琐，尤其是需要配置复杂请求body，请求头，参数的时候
###### (2).数据解析过程需要用户手动拿到responsebody进行解析，不能复用
###### (3).无法适配自动进行线程的切换
###### (4).万一我们存在嵌套网络请求就会陷入“回调陷阱”
##### 2.Retrofit封装的点
######(1).okhttp创建的是OkhttpClient,然而retrofit创建的是Retrofit实例
######(2).构建Request的方案，retrofit是通过注解来进行适配
######(3).配置call的过程中，retrofit是利用Adapter适配的Okhttp的call，为Call的适配提供了多样性
######(4).相对okhttp,retrofit回怼responseBody进行自动的Gson解析，提供了可复用，易扩展的数据解析方案
###### (5).相对okhttp，retrofit会自动的完成线程的切换
##### 3.Build模式什么情况下使用
参数大于5个，存在可选参数
##### 4.成功建立一个Retrofit对象的标准: 配置好Retrofit类里的成员变量
baseUrl:网络请求的URL地址。callFactory: 网络请求工厂（默认是okhttp）。callbackExecutor:回调方法执行器(默认是MainThreadExecutor,通过它进行线程间切换)。adapterFactory:网络请求适配器工厂的集合（将okhttp call 转换成retrofit call）。converFactories:数据转换器工厂的集合
##### 5.retrofit.java 使用的设计模式
外观：门面模式
##### 6.onCreate()方法中 
使用的是动态代理模式
##### 7.ServiceMethod 
构建一个具体的网络请求
##### 8.rxjava 作用
防止回调陷阱，管理流程 
