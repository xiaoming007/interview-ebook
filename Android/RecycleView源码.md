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
##### 2.RecycleView 源码跟踪路径
###### (1).onTouchEvent -> scrollByInternal -> scrollStep
###### (2).mLayout.scrollVerticallyBy -> scrollBy -> fill -> layoutChunk
###### (3).layoutState.next -> recycler.getViewForPosition
###### (4).tryGetViewHolderForPositionByDeadline
####### 1).getChangedScrapViewForPosition
####### 2).getScrapOrHiddenOrCachedHolderForPosition
####### 3).getScrapOrCachedViewForId
####### 4).mViewCacheExtension.getViewForPositionAndType -- 自定义
####### 5).getRecycledViewPool().getRecycledView(type) -- 缓存池
####### 6).mAdapter.createViewHolder -- 创建
####### 7).tryBindViewHolderByDeadline -- 绑定数据
##### 3.RecycleView.Recycler 
处理缓存复用
##### 4.缓存代码跟踪
```java
LinearLayoutManager.onLayoutChildren 
-> detachAndScrapAttachedViews 
-> scrapOrRecycleView 
-> recycler.recycleViewHolderInternal(viewholder) 
-> recycler.scrapView(view)
```
