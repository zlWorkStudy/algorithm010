#### 二分查找一个半有序数组中间无序的地方
```java
/**
 * 思路：
 *      mid = (left + right) / 2;
 *      mid < nums[right], right = mid;
 *      mid > nums[left], left = mid
 */
```