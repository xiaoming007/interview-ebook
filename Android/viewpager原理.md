# Viewpager的缓存机制原理
#### 1.RecycleView是有Filling事件的，有惯性滚动效果，但是ViewPager2没有，这是怎么实现的
##### (1).发生Fling事件，阻止RecyclerView的Fling事件
SnapHelper类处理View的Fling事件，在onFling()方法中会调用snapFromFling()方法来确定返回值，snapFromFling()方法会调用findTargetSnapPosition()这个抽象方法，有PageSnapHelper类实现findTargetSnapPosition()方法供其调用，需要Fling，所以返回了一个具体的targetPosition,然后通过smollthScroll 滑动到上一个页面，或者下一个页面
##### (2).页面不发生Fling事件时，页面滑动一半时松手后的回弹处理
会回调SnapHelper里注册的RecyclerView.OnScrollListener滑动监听里的滑动状态变化监听方法，此时Recycleview的状态为IDLE,所以执行snapToTargetExistingView()方法，在这个方法里，首先调用findSnapView()方法找到要滑动的view，然后通过这个view参数，调用calculateDistanceToFinalSnap()方法计算出需要滑动的距离，最后调用mRecyclerView.smoothScrooBy()方法滑动到具体的位置
##### (3).滑动位置的计算
遍历所有子view，找到屏幕的中心位置，找到view的中心位置，相减，比较然后取出相减后值最小的那个view，同理获取这个view的中心位置，减去父view的中心位置，就是需要滑动的距离，有正负之分，就是滑动方向
##### (4).有一个问题需要解决，RecyclerView滑动后画布的高度或者宽度问题
#### 2.RecyclerView如何加载Fragment？
查看第七条
#### 3.ViewPager2滑动到最后一个tab默认会销毁之前的fragment吗
##### (1).Prefetch机制
(view_pager.getChildAt(0) as RecyclerView).layoutManager!!.isItemPrefetchEnabled = false,设置之后默认缓存与当前页面最近的两个页面。在GapWork 线程中做一些操作，流程RecyclerView的onTouchEvent() ->postFromTraversal() -> buildTaskList() -> collectPrefectchPositionsFromView()
#### 4.ViewPager2默认不会进行提前加载
offscreenPageLimit默认是0，不会提前加载下一个Fragment默认是缓存所有Fragment，不会去销毁它
#### 5.PageTransformerAdapter
##### (1).MarginPageTransformer
viewpager2移除了setPageMargin方法，设置页面间距使用MarginPageTransformer
##### (2).CompositePageTransformer
内部维护了一个集合可以向它添加多个Transformer比如动画了，间距了等等
#### 6.ScrollEventAdapter
将RecyclerView的滚动事件转换成了Viewpager2的事件
#### 7.FragmentStateAdapter
##### (1).onCreateViewHolder()
很简单就是创建FrameLayout视图，就不多说
##### (2).onBindViewHolder()
如果当前ItemView上已经加载了Fragment，并且不是同一个Fragment(ItemView被复用了)，那么先移除掉ItemView上的Fragment。ensureFragment()初始化Fragment视图  
##### (3).onViewAttachedToWindow (ViewHolder与Fragment进行绑定)
调用了placeFragmentInViewHolder方法加载Fragment,和gcFragments的操作
##### (4).onViewRecycled (ViewHolder与Fragment解除绑定)
当ViewHolder被回收到回收池中，onViewRecycled方法会被调用,这里重要的一点是没有在onViewDetachedFromWindow中去删除Fragment,而是在onViewRecycled中回收。因为当onViewRecycled方法被调用，表示当前ViewHolder已经彻底没有用了，被放入回收池，等待后面被复用，此时存在的情况可能有：(1).当前ItemView手动移除掉了；(2).当前位置对应的视图已经彻底不在屏幕中，被当前屏幕中某些位置复用了。所以在onViewRecycled方法里面移除Fragment比较合适 2.不在onViewDetachedFromWindow中删除的原因呢? 因为每当一个页面被滑走，都会调用这个方法，如果对其Fragment进行卸载，此时用户又滑回来，又要重新加载一次，这性能就下降了很多
##### (5).placeFragmentInViewHolder
viewPager2的Lifecycle跟ViewPager一样默认是(setMaxLifecycle(fragment, STARTED) 懒加载)
##### (6).onPageScrollStateChanged
滑动界面展示是滑动界面时设置为Resume更新状态
