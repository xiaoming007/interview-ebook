# RecycleView源码分析
##### 1.四级缓存
###### (1).mChangeScrap与mAttachedScrap
用来缓存还在屏幕内的ViewHolder
###### (2).mCachedViews
用来缓存移除屏幕之外的ViewHolder
###### (3).mViewCacheExtension
这个的创建和缓存完全由开发者自己控制，系统未往这里添加数据
###### (4).RecycleViewPool
ViewHolder缓存池
##### 2.RecycleView 源码跟踪路径(入口分析，因为只有在列表滑动的时候才会出现缓存复用问题，所以入口在onTouchEvent()中的Move事件监听那里)
###### (1).onTouchEvent -> scrollByInternal -> scrollStep
###### (2).mLayout.scrollVerticallyBy -> scrollBy -> fill -> layoutChunk
###### (3).layoutState.next -> recycler.getViewForPosition
###### (4).tryGetViewHolderForPositionByDeadline 缓存复用的关键代码（在Recycler类里）
####### 1).getChangedScrapViewForPosition（从mChangeScrap中拿到viewhold）
####### 2).getScrapOrHiddenOrCachedHolderForPosition（如果第一步未拿到，从这个方法中取）
####### 3).getScrapOrCachedViewForId
####### 4).mViewCacheExtension.getViewForPositionAndType -- （自定义缓存，自己使用）
####### 5).getRecycledViewPool().getRecycledView(type) -- 缓存池，如果缓存池中也没有就执行下一步创建 
####### 6).mAdapter.createViewHolder -- 创建
####### 7).tryBindViewHolderByDeadline -- 绑定数据
##### 3.RecycleView.Recycler 
处理缓存复用
##### 4.缓存代码跟踪
```java
LinearLayoutManager.onLayoutChildren 
-> detachAndScrapAttachedViews 
-> scrapOrRecycleView 
-> recycler.recycleViewHolderInternal(viewholder) 缓存到 mCachedViews(大小2)，RecycledViewPool(大小5) ==> 7 
-> recycler.scrapView(view)一级缓存，缓存mChangeScrap与mAttachedScrap
```
##### 5.RecyclerViewPool
结构是一个类似HashMap的结构，key是view_type后面跟的一个链表，使用SparseArray,如果链中数据满了直接reture掉，不需要那么多的ViewHold.
#### RecyclerView 相关面试问题
##### 1.RecyclerView的复用机制，简单说说View回收与复用的过程

##### 2.RecyclerView支持多个不同类型布局，他们怎么缓存，并且查找的呢

##### 3.说说RecyclerView适配器的原理

##### 4.理清RecyclerView架构思想，手写RecyclerView自定义控件

##### 5.RecyclerView中每一个View的tag是空的吗
不空，系统会为其设置tag

##### 6.RecyclerView一屏加载的个数是怎么确定的，他是怎么做到不显示的item缓存到内存中
每一个item都有一个top,和bottom，bottom不断的增加，当其值大于等于可用高度时就不在添加item了
##### 7.有看过RecyclerView的边界判断的源码吗？简短聊下他的判断机制，什么时候该回收
