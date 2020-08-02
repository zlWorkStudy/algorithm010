#### XOR异或  ^
```
x ^ 0 = x
# 1s = ~0
x ^ 1s = ~x
x ^ (~x) = 1s
x ^ x = 0
c = a ^ b  -->  a ^ c = b  -->  b ^ c = a
a ^ b ^ c = a ^ (b ^ c) = (a ^ b) ^ c
```
<br/>

### 指定位置的位运算
```
# 将x最右边的n位清零
x & (~0 << n)
# 获取x第n位的值(0 or 1)
x >> n & 1
# 获取第n位的幂值
x & (1 << n)
# 仅将第n位置为 1
x | (1 << n)
# 仅将第n位置为 0
x & (~(1 << n)
# 将x最高位至n位(含n)清零
x & ((1 << n) - 1)
# 将x第n位至0位(含n)清零
x & (~((1 << n + 1) - 1))
```
<br/>

### 实战运用
```
# 清零最低位 1
X = X & (X - 1)
# 得到最低位 1
X = X & -X
```
<br/>

### 布隆过滤器
```
# 核心: 
    超大位数组 + hash函数

# 添加元素
    1. 将添加元素给k个hash函数
    2. 得到对应位数组的k个位置
    3. 将对应位置设为 1
# 查询元素
    1. 将下旬元素给k个hash函数
    2. 得到对应位数组的k个位置
    3. 只要有一个位置值为 0, 则元素不存在
    3. 如果k个位置值全为 1, 则元素可能存在(存在误判, 用于最外层过滤)
```

```
/**
 * 示例代码
 */
public class BloomFilter {
    private static final int DEFAULT_SIZE = 2 << 24;
    private static final int[] seeds = new int[] { 5, 7, 11, 13, 31, 37, 61 };
    private BitSet bits = new BitSet(DEFAULT_SIZE);
    private SimpleHash[] func = new SimpleHash[seeds.length];
    public BloomFilter() {
        for (int i = 0; i < seeds.length; i++) {
            func[i] = new SimpleHash(DEFAULT_SIZE, seeds[i]);
        }
    }
    public void add(String value) {
        for (SimpleHash f : func) {
            bits.set(f.hash(value), true);
        }
    }
    public boolean contains(String value) {
        if (value == null) {
            return false;
        }
        boolean ret = true;
        for (SimpleHash f : func) {
            ret = ret && bits.get(f.hash(value));
        }
        return ret;
    }
    // 内部类，simpleHash
    public static class SimpleHash {
        private int cap;
        private int seed;
        public SimpleHash(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }
        public int hash(String value) {
            int result = 0;
            int len = value.length();
            for (int i = 0; i < len; i++) {
                result = seed * result + value.charAt(i);
            }
            return (cap - 1) & result;
        }
    }
}
```
<br/>

### LRU Cache
```java
/**
 * 示例代码
 */
class LRUCache {
    /**
     * 缓存映射表
     */
    private Map<Integer, DLinkNode> cache = new HashMap<>();
    /**
     * 缓存大小
     */
    private int size;
    /**
     * 缓存容量
     */
    private int capacity;
    /**
     * 链表头部和尾部
     */
    private DLinkNode head, tail;

    public LRUCache(int capacity) {
        //初始化缓存大小，容量和头尾节点
        this.size = 0;
        this.capacity = capacity;
        head = new DLinkNode();
        tail = new DLinkNode();
        head.next = tail;
        tail.prev = head;
    }

    /**
     * 获取节点
     * @param key 节点的键
     * @return 返回节点的值
     */
    public int get(int key) {
        DLinkNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        //移动到链表头部
         (node);
        return node.value;
    }

    /**
     * 添加节点
     * @param key 节点的键
     * @param value 节点的值
     */
    public void put(int key, int value) {
        DLinkNode node = cache.get(key);
        if (node == null) {
            DLinkNode newNode = new DLinkNode(key, value);
            cache.put(key, newNode);
            //添加到链表头部
            addToHead(newNode);
            ++size;
            //如果缓存已满，需要清理尾部节点
            if (size > capacity) {
                DLinkNode tail = removeTail();
                cache.remove(tail.key);
                --size;
            }
        } else {
            node.value = value;
            //移动到链表头部
            moveToHead(node);
        }
    }

    /**
     * 删除尾结点
     *
     * @return 返回删除的节点
     */
    private DLinkNode removeTail() {
        DLinkNode node = tail.prev;
        removeNode(node);
        return node;
    }

    /**
     * 删除节点
     * @param node 需要删除的节点
     */
    private void removeNode(DLinkNode node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }

    /**
     * 把节点添加到链表头部
     *
     * @param node 要添加的节点
     */
    private void addToHead(DLinkNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    /**
     * 把节点移动到头部
     * @param node 需要移动的节点
     */
    private void moveToHead(DLinkNode node) {
        removeNode(node);
        addToHead(node);
    }

    /**
     * 链表节点类
     */
    private static class DLinkNode {
        Integer key;
        Integer value;
        DLinkNode prev;
        DLinkNode next;

        DLinkNode() {
        }

        DLinkNode(Integer key, Integer value) {
            this.key = key;
            this.value = value;
        }
    }
}
```
<br/>

### <a href="https://www.cnblogs.com/onepixel/p/7674659.html">排序算法</a>

#### 冒泡排序
```java
public void bubbleSort(int[] nums) {
    int len = nums.length;
    // 相邻两个数比较, 顺序错误则交换位置
    // time:O(n ^ 2)        space:O(1)
    for (int i = 0; i < len; i++) {
        for (int j = 0; j < len - 1 - i; j++) {
            if (nums[j] > nums[j + 1]) {
                int temp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = temp;
            }
        }
    }
}
```
<br/>

#### 选择排序
```java
public void selectionSort(int[] nums) {
    int len = nums.length;
    // 在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
    // time:O(n ^ 2)        space:O(1)
    for (int i = 0; i < len; i++) {
        int minIndex = i;
        for (int j = i + 1; j < len; j++) {
            // 找从i开始后的最小数的索引
            if (nums[j] < nums[minIndex]) minIndex = j;
        }
        // 交换位置
        int temp = nums[i];
        nums[i] = nums[minIndex];
        nums[minIndex] = temp;
    }
}
```
<br/>

#### 插入排序
```java
public void selectionSort(int[] nums) {
    int len = nums.length;
    // 构建有序序列, 对于未排序数据, 在已排序序列中从后向前扫描, 找到相应位置并插入
    // time:O(n ^ 2)        space:O(1)
    for (int i = 0; i < len; i++) {
        int curNum = nums[i], curIndex = i - 1;
        while (curIndex >= 0 && nums[curIndex] > curNum) {
            nums[curIndex + 1] = nums[curIndex];
            curIndex--;
        }
        nums[curIndex + 1] = curNum;
    }
}
```
<br/>

#### 快速排序
```java
/**
 * 通过一趟排序将待排记录分隔成独立的两部分，
 *     其中一部分记录的关键字均比另一部分的关键字小，
 *     则可分别对这两部分记录继续进行排序，以达到整个序列有序
 */
public void quickSort(int[] nums) {
    // time:O(nlog n)       space:O(nlog n)
    quickSort(nums, 0, nums.length - 1);
}

private void quickSort(int[] nums, int begin, int end) {
    if (begin <= end) return;
    // 寻找标杆位置
    int pivot = partition(nums, begin, end);
    // pivot左边元素下探
    quickSort(nums, begin, pivot - 1);
    // pivot右边元素下探
    quickSort(nums, pivot + 1, end);
}

private int partition(int[] nums, int begin, int end) {
    // pivot: 标杆位置, counter: 小于pivot的元素的个数
    int pivot = end, counter = begin;
    for (int i = begin; i < end; i++) {
        if (nums[i] < nums[pivot]) {
            int temp = nums[i]; nums[i] = nums[counter]; nums[counter] = temp;
            counter++;
        }
    }
    int temp = nums[pivot]; nums[pivot] = nums[counter]; nums[counter] = temp;
    return counter;
}
```
<br/>

#### 归并排序
```java
/**
 * 采用分治法（Divide and Conquer）的一个非常典型的应用。
 * 将已有序的子序列合并，得到完全有序的序列；
 * 即先使每个子序列有序，再使子序列段间有序
 */
public void mergeSort(int[] nums) {
    // time:O(nlog n)       space:O(n)
    mergeSort(nums, 0, nums.length - 1);
}

private void mergeSort(int[] nums, int left, int right) {
    if (left <= right) return;
    int mid = ((right - left) >> 1) + left;
    // 左半部分下探
    mergeSort(nums, left, mid);
    // 右半部分下探
    mergeSort(nums, mid + 1, right);
    merge(nums, left, mid, right);
}

/**
 * 合并两个有序数组
 */
private int merge(int[] nums, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        temp[k++] = nums[i] < nums[j] ? nums[i++] : nums[j++];
    }
    while (i <= left) temp[k++] = nums[i++];
    while (j <= right) temp[k++] = nums[j++];
    System.arraycopy(temp, 0, nums, left, right - left + 1);
}
```