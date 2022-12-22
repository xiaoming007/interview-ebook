# Activity,View,Window
源码分析：主要有两个类Activity.java,PhoneWindow.java。从activity的setContentView方法入手，会看到，该方法是调用的PhoneWindow中的setContentView方法，该方法通过mLayoutInflater.inflate(layoutResID, mContentParent),将布局设置上去，如果mContentParent为空，会初始化一个DecorView对象并赋值给mContentParent。
### 1.Activity
是存放View的容器，也是我们界面的载体，通过setContentView()方法展示定义的布局
### 2.view
就是一个视图对象
### 3.Window
是一个抽象类，唯一的实例PhoneWindow,它提供标准的用户界面策略，如背景，标题，区域，默认按键处理等。
