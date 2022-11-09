# 自定义view
##### 1. onMeasure()方法的使用
###### (1).measure()方法中循环遍历孩子view，确定孩子的宽高
###### (2).
###### (2).
###### (2).
###### (2).
##### 2.MeasureSpec 是什么
它是一个32位的int值，其中前两位是测量模式，低30位为具体的测量大小，根据父亲给的大小和测量的孩子大小，确定自己的大小
##### 3.测量主要针对的是系统提供的match_parent,wrap_content
##### 4.三种mode
exactly: 精确值模式，一般是childview设置了宽高值，或者match_parent,at_most:表示子view被限制在一个最大值内，wrap_content,unspecifide
##### 5.两种相对坐标
相对与屏幕的坐标系，相对于父view的坐标系
##### 6.view的生命周期
###### (1).构造方法 -> view初始化
###### (2).onMeasure() -> 测量view大小
###### (3).onSizeChanged() -> 确定view大小
###### (4).onLayout() -> 确定子view布局（包含子view时用）
###### (5).
###### (6).onDraw() -> 实际绘制内容
###### (7).视图状态改变 -> 用户操作或者自身变化引起，触发（8）
###### (8).invalidate() -> 根据条件判断是否需要执行（2）或者（4）或者（6）
##### 7.getMeasureWidth与getWidth的区别
###### (1).getMeasureWidth()
在measure()过程结束后就可以获取到对应的值，通过setMeasureDimension()方法进行设置的
###### (2).getWidth
在layout()过程结束后才能获取到，通过视图右边的坐标减去左边的坐标计算出来的



