### 树
```
父节点中有所有子节点的指针
    前序遍历
    后序遍历
    层序遍历
```

### 二叉树
```
父节点中有左右子节点的指针
    前序遍历 -- 根-左-右
    中序遍历 -- 左-根-右
    后续遍历 -- 左-右-根
    层序遍历
```

### 二叉搜索树
```
1.左节点小于根节点
2.根节点小于右节点
中序遍历为升序排列
```

### 堆
```
常见的有二叉堆(完全二叉树)，斐波那契堆等
    一般用于找最值，PriorityQueue有二叉堆实现
    1.根为最大值：大顶堆
        父节点 >= 子节点
    2.根为最小值：小顶堆
        父节点 <= 子节点
若父节点索引为i：
    左节点索引：2 * i + 1   or  i << 1 + 1
    右节点索引：2 * i + 1   or  i << 1 + 2
若子节点序索引i：
    父节点索引：(i - 1) / 2 or  (i - 1) >> 1
```

### HashMap
#### 常量
```
链表+数组(红黑树)
```

```java
// 默认的初始容量是16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;   
// 最大容量
static final int MAXIMUM_CAPACITY = 1 << 30; 
// 默认加载充因子
static final float DEFAULT_LOAD_FACTOR = 0.75f;
// 当桶(bucket)上的结点数大于这个值时会转成红黑树
static final int TREEIFY_THRESHOLD = 8; 
// 当桶(bucket)上的结点数小于这个值时树转链表
static final int UNTREEIFY_THRESHOLD = 6;
// 桶中结构转化为红黑树对应的table的最小大小
static final int MIN_TREEIFY_CAPACITY = 64;
// 存储元素的数组，总是2的幂次倍
transient Node<k,v>[] table; 
// 存放具体元素的集
transient Set<map.entry<k,v>> entrySet;
// 存放元素的个数，注意这个不等于数组的长度。
transient int size;
// 每次扩容和更改map结构的计数器
transient int modCount;   
// 临界值 当实际大小(容量*加载因子)超过临界值时，会进行扩容
int threshold;
// 加载因子
final float loadFactor;
```

#### putVal
```
1.计算hash值，确定在table中的位置
2.将元素插入指定位置
    1) 如果没有元素，直接插入
        a.插入后，数组元素超过扩容阈值(threshold)，对table进行扩容
        b.插入后，小于threshold，结束
    2) 如果有元素，挂在已有元素后
        a.如果之前已形成树，直接插入树对应的位置
        b.如果之前元素组成链表
            a) 加入元素后链表长度 > 8，将链表转为红黑树插入
            b) 加入元素后链表长度 <= 8, 直接插入
```

#### get
```
1.根据key，hash计算出存放在数组中的位置
2.取出指定位置的元素
    1) key完全相同，取出对应的value
    2) 若key不相同，判断挂载的是链表还是红黑树
        a. 若为树，按照树的方法查找
        b. 若为链表，按照链表的方法查找
```