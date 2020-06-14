## 数据结构时间复杂度
<br/>

|  类型  |   add  | remove |  query | modify | peculiarity |
| :----: | :----: | :----: | :----: | :----: | :----: |
| Array  | O(n) | O(n) | O(1) | O(1) | 随机加速访问 |
| Linked List | O(1) | O(1) | O(n) | O(1) | next指针 |
| Double Linked List | O(1) | O(1) | O(n) | O(1) | pre,next指针 |
| Skip List | O(logn) | O(logn) | O(logn) | -- | 元素有序 |
| Stack | O(1) | O(1) | O(n) | -- | 后进先出 |
| Queue | O(1) | O(1) | O(n) | -- | 先进先出 |
| Deque | O(1) | O(1) | O(n) | -- | 两端都可进出的Quue |
| Priority Queue | O(1) | -- | O(logn) | -- | 两端都可进出的Quue |
<br/>

## PriorityQueue分析
```
balanced binary heap:
	父结点的键值总是小于或等于任何一个子节点的键值
	基于数组实现的二叉堆，对于数组中任意位置的n上元素，其左孩子在[2n+1]位置上，右孩子[2(n+1)]位置，它的父亲则在[n-1/2]上，而根的位置则是[0]
fields:
	comparator--比较器，无此参数元素使用自然排序
	queue--数组(存数据)
	size--元素个数
method:
	#grow -- 数组扩容
	#hugeCapacity -- 容量:0-Integer.MAX_VALUE
	#add -- 调用offer(E e)
	#offer -- size >= queue.length (扩容)，size == 0直接添加为根，否则从队尾开始上移 --> siftUp
	#siftUp
		#siftUpComparable -- 一直上移至找到父节点
		#siftUpUsingComparator -- 操作类似，自然排序
	#peek -- 查看队列首元素，空时返回null
	#indexOf -- 查看元素对应下标
	#remove -- 获取要移除元素下标i，--size,提取e = queue[size],queue[size] = null, i < size / 2将queue[i]不断下移，否则上移
	#poll -- 移除根， --size,提取e = queue[size],queue[size] = null，queue[i]不断下移，直到queue[i]的最小孩子比e 大为止
	#siftDown
		#siftDownComparable	-- 以k为父节点找子节点(left, right),c = queue[left], 
				如果left < right && queue[left] > queue[right] { c = queue[right] },交换父节点和c;直至队尾元素比c的子节点都要小
		#siftDownUsingComparator -- 操作类似，自然排序
```