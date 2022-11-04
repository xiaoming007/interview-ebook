# Context
##### 1.Dialog为什么不能使用ApplicationContext
因为dialog是子window需要依附于主window上，而ApplicationContext没有window
##### 2.Activity启动为什么必须要设置启动参数
因为Activity是放置在Activity栈里的
