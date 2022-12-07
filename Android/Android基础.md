# android基础知识
### 1.当回到原来Activity时
onRestart->onStart->onResume
### 2.启动模式
* Standard：标准模式，也是默认模式。每次启动都会创建一个全新的实例
* SingleTop：栈顶复用模式。这种模式下如果Activity位于栈顶，不会新建实例。onNewIntent会被调用，接收新的请求信息，不会再低啊用onCreate和onStart
* SingleTask：栈内复用模式。升级版singleTop，如果栈内有实例，则复用，并会将该实例之上的Activity全部清除
* SingleInstance：系统会为它创建一个单独的任务栈，并且这个实例独立运行在一个 task中，这个task只有这个实例，不允许有别的Activity 存在（可以理解为手机内只有一个）
### 3.Fragment
* onAttach()：Fragment和Activity相关联时调用。如果不是一定要使用具体的宿主 Activity 对象的话，可以使用这个方法或者getContext()获取 Context 对象，用于解决Context上下文引用的问题。同时还可以在此方法中可以通过getArguments()获取到需要在Fragment创建时需要的参数。
* onCreate()：Fragment被创建时调用。
* onCreateView()：创建Fragment的布局。
* onActivityCreated()：当Activity完成onCreate()时调用。
* onStart()：当Fragment可见时调用。
* onResume()：当Fragment可见且可交互时调用。
* onPause()：当Fragment不可交互但可见时调用。
* onStop()：当Fragment不可见时调用。
* onDestroyView()：当Fragment的UI从视图结构中移除时调用。
* onDestroy()：销毁Fragment时调用。
* onDetach()：当Fragment和Activity解除关联时调用。
### Service
#### 1.启动方式
Service的启动方式主要有两种，分别是startService和bindService。使用startService启动时是单独开一个服务，与Activity没有任何关系，而bindService方式启动时，Service会和Activity进行绑定，当对应的activity销毁时，对应的Service也会销毁。其中，StartService使用的是同一个Service，因此onStart()会执行多次，onCreate()只执行一次，onStartCommand()也会执行多次。使用bindService启动时，onCreate()与onBind()都只会调用一次
#### 2.生命周期
startService onCreate() -> onStartCommand()-> onBind()
### BroadcastReceiver
广播的注册分为静态注册和动态注册。静态注册是在Mainfest清单文件中进行注册.动态注册是在代码中，使用registerReceiver方法代码进行注册。差别在注册的时机，动态注册的，只有在注册后才能接收
### ContentProvider
内容提供者，进程间传递数据用的，底层还是binder
