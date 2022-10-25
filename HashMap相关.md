# HashMap
#### 1.底层数据结构（红黑树 + list）
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
#### 11.SparseArray源码
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
