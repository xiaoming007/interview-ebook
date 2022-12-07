# Android设备启动流程
### 一.Android系统架构
andorid系统架构从上到下分别有应用层、应用框架层、系统运行时库层、硬件抽象层、Linux内核层。
### 二.系统的启动流程解析
#### 1.启动电源以及系统启动
加载引导程序Bootloader到RAM，然后执行。
#### 2.引导程序BootLoader启动
引导程序BootLoader是在Android操作系统开始运行前的一个小程序，它的主要作用是把系统OS拉起来并运行。
#### 3.Linux内核启动
内核启动时，设置缓存、被保护存储器、计划列表、加载驱动。当内核完成系统设置，它首先在系统文件中寻找init.rc文件，并启动init进程。
#### 4.init进程启动
init进程是系统空间内的第一个进程，进行初始化和启动属性服务，在main方法中进行，包括初始化资源文件和启动一系列的属性服务。通过执行init.rc文件的脚本文件来启动Zygote进程。
#### 5.Zygote进程启动
所有的应用程序包括system系统进程 都是zygote进程负责创建，因此zygote进程也被称为进程孵化器，它创建进程是通过复制自身来创建应用进程，它在启动过程中会在内部创建一个虚拟机实例，所以通过复制zygote进程而得到的应用进程和系统服务进程都可以快速地在内部的获得一个虚拟机实例拷贝。
#### 6.SystemServer进程启动
启动Binder线程池和SystemServiceManager，systemServiceManger主要是对系统服务进行创建、启动和生命周期管理，就会启动各种系统服务。（android中最核心的服务AMS就是在SystemServer进程中启动的）
#### 7.Launcher启动
Launcher组件是由之前启动的systemServer所启动的
这也是andorid系统启动的最后一步，launcher是andorid系统home程序，主要是用来显示系统中已安装的应用程序。 launcher应用程序的启动会通过请求packageManagerService返回系统中已经安装的应用信息，并将这些应用信息通过封装处理成快捷列表显示在系统屏幕上，这样咱们就可以单击启动它们。
### 三.应用的启动流程
* （1）Launcher首先向activityManagerService发送一个启动activity的进程间通信请求
* （2）ams会先把要启动的activity信息保存下来，然后再想Launcher发送一个进入中止状态的进程间通信请求
* （3）Launcher组件进入终止状态后，就会给ams发送一个已进入终止状态的一个进程间通信请求，ams收到后就会继续执行启动activity操作
* （4）ams如果发现用来运行运行activity的进程不存在，它就会给zygote进程发送一个进程间通信请求，zaygote会调用fork（）方法创建一个新的应用程序进程。zaygote进程在启动的时候在内部创建一个虚拟机实例，它通过复制它本身得到一个应用程序进程
* （5）新的应用程序进程启动完成之后，就会向ams发送一个启动完成的通信请求
* （6）进程创建好之后，经过一系列调用就会调用startactivity方法，最后 activity调用oncreate方法构建出页面至此我们的应用正式启动完成
