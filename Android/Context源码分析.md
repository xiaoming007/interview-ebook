# Context
### Context是什么
* 理解为“上下文”：它贯穿整个应用
* 理解成“运行环境”：它提供了一个应用运行所需要的信息，资源，系统服务等
* 理解成“场景”：用户操作和系统交互这一过程就是一个场景，比如Activity之间的切换，服务的启动等都少不了Context  
### Application，service，activity与Context的继承关系
使用静态代理模式设计的，Context抽象类有两个继承者，一个是ContextWrapper，一个是ContextImpl，其中ContextWrapper是一个代理类，Service和Application都继承它，并且会创建一个ContextImpl的实例，通过构造方法传递给ContextWrapper，实现静态代理模式  
* Context 是一个抽象类
* ContextImpl：具体的实现类比如startActivity等
* ContextWrapper：代理类，Service，Application，都直接继承它，Activity通过ContextThemeWrapper间接的继承它
* ContextThemeWrapper：由于Activity比较特殊，它需要展示页面需要主题，而Service和Application不需要，所以中间又加了一层  

### Application和Service不能直接启动一个activity
因为activity被启动后，需要放入一个activity任务栈里，但是Application和Service没有依赖的Activity任务栈，所以不能直接启动activity，如果想要启动一个activity，需要设置标志FLAG_ACTIVITY_NEW_TASK,创建一个新的activity栈来放置这个activity
### 除了activity上下文以外都不能弹出Dilog
引文Dialog是一个子Window它需要添加到主window上才能展示出来
### 尽量少用Context对象去获取静态变量，静态方法，以及单例对象，以免导致内存泄漏
### getApplication和getApplicationContext的区别
内部调用的是一个获取context的方法，只是他们的实现位置不同
