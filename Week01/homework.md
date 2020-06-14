## Week01 homework
<br>

### 删除排序数组中的重复项
```java
public int removeDuplicates(int[] nums) {
    if (nums == null && nums.length == 0) {
        return 0;
    }
    int left = 0;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] != nums[left]) {
            nums[++left] = nums[i];
        }
    }
    return ++left;
}
```
<br/>

### 旋转数组
```java
public void rotate(int[] nums, int k) {
    // 使用环状替代，k %= nums.length
    k %= nums.length;
    int count = 0;
    for (int start = 0; count < nums.length; start++) {
        int current = start;
        int pre = nums[start];
        do {
            int next = (current + k) % nums.length;
            int temp = nums[next];
            nums[next] = pre;
            pre = temp;
            current = next;
            count++;
        } while (start != current);
    }
}
```
<br/>

### 合并两个有序链表
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode preHead = new ListNode(-1);
    ListNode pre = preHead;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            pre.next = l1;
            l1 = l1.next;
        } else {
            pre.next = l2;
            l2 = l2.next;
        }
        pre = pre.next;
    }
    pre.next = l1 == null ? l2 : l1;
    return preHead.next;
}
```
<br/>

### 合并两个有序数组
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    while (m > 0 && n > 0) {
        nums1[m + n - 1] = nums1[m - 1] > nums2[n - 1] ? nums1[--m] : nums2[--n];
    }
    for (int i = 0; i < n; i++) {
        nums1[i] = nums2[i];
    }
}
```
<br/>

### 两数之和
```java
public int[] twoSum(int[] nums, int target) {
    // 循环遍历数组，num[i]存在于map中，直接返回map中的value和i
    // 否则，将key:target - nums[i]     value:i 存入map中
    int length = nums.length;
    Map<Integer, Integer> map = new HashMap<>(length);
    for (int i = 0; i < length; i++) {
        if (map.containsKey(nums[i])) {
            return new int[]{map.get(nums[i]), i};
        }
        map.put(target - nums[i], i);
    }
    return null;
}
```
<br/>

### 移动零
```java
public void moveZeroes(int[] nums) {
    if (nums == null) {
        return;
    }
    int index = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            if (index != i) {
                nums[index] = nums[i];
                nums[i] = 0;
            }
            index++;
        }
    }
}
```

### 加一
```java
public int[] plusOne(int[] digits) {
    // digits数组中所有位全进1，则新new一个digits.length + 1长度的数组，并把首位置为1
    for (int i = digits.length - 1; i >= 0; i--) {
        digits[i] += 1;
        if (digits[i] % 10 != 0) {
            return digits;
        }
        digits[i] = 0;
    }
    digits = new int[digits.length + 1];
    digits[0] = 1;
    return digits;
}
```