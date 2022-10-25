# HashMap
#### 1.put()（红黑树 + 链表 + 数组）
```bash
  public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
   final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
             //如果table尚未初始化，则此处进行初始化数组，并赋值初始容量，重新计算阈值
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            //通过hash找到下标，如果hash值指定的位置数据为空，则直接将数据存放进去
            tab[i] = newNode(hash, key, value, null);
        else {
            //如果通过hash找到的位置有数据，发生碰撞
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                //如果需要插入的key和当前hash值指定下标的key一样，先将e数组中已有的数据
                e = p;
            else if (p instanceof TreeNode)
                //如果此时桶中数据类型为 treeNode，使用红黑树进行插入
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                //此时桶中数据类型为链表
                // 进行循环
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        //如果链表中没有最新插入的节点，将新放入的数据放到链表的末尾
                        p.next = newNode(hash, key, value, null);

                        //如果链表过长，达到树化阈值，将链表转化成红黑树
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    //如果链表中有新插入的节点位置数据不为空，则此时e 赋值为节点的值，跳出循环
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }

            //经过上面的循环后，如果e不为空，则说明上面插入的值已经存在于当前的hashMap中，那么更新指定位置的键值对
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        //如果此时hashMap size大于阈值，则进行扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

```
- （1）树化条件：table容量大于最小树化容量，并且链表长度大于树化阈值。

#### 2.hashCode的生成
#### 3.hash 碰撞问题
#### 4.扩容问题
阈值：
装载因子：
#### 5.初始容量为16
#### 6.红黑树基本概念
#### 7.时间复杂度
#### 8.线程安全问题（线程不安全的）
#### 9.hashTable是线程安全（函数锁 synchronized）
#### 10.ConcurrentHashMap效率比较高线程安全

### SparseArray源码
###### (1).二分查找其key,这里key必须是int的
```bash
// This is Arrays.binarySearch(), but doesn't do any argument validation.
    static int binarySearch(int[] array, int size, int value) {
        int lo = 0;
        int hi = size - 1;

        while (lo <= hi) {
            final int mid = (lo + hi) >>> 1;
            final int midVal = array[mid];

            if (midVal < value) {
                lo = mid + 1;
            } else if (midVal > value) {
                hi = mid - 1;
            } else {
                return mid;  // value found
            }
        }
        return ~lo;  // value not present
    }
```
