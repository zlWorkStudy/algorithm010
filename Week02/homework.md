### 有效的字母异位词
```
异位词：
    两个字符串s与t，s和t相同字母出现次数相同
```

```java
/**
 * 暴力
 */
public boolean isAnagram(String s, String t) {
    if (s == null || t == null || s.length() != t.length()) {
        return false;
    }
    // 字符串排序，比较字符串是否相等
    char[] ss = s.toCharArray();
    char[] tt = t.toCharArray();
    Arrays.sort(ss);
    Arrays.sort(tt);
    return Arrays.equals(ss, tt);
}
```
<br/>

```java
public boolean isAnagram(String s, String t) {
    if (s == null || t == null || s.length() != t.length()) {
        return false;
    }
    /*
     * 只有小写字母(a-z) 26位，数组result存字母出现次数的计数器
     *     1.当s中出现该字母，则对应下标值+1
     *     2.当t中出现该字母，则对应下标值-1
     */
    int[] result = new int[26];
    for (int i = 0; i < s.length(); i++) {
        result[s.charAt(i) - 'a']++;
    }
    for (int i = 0; i < t.length(); i++) {
        result[t.charAt(i) - 'a']--;
        if (result[t.charAt(i) - 'a'] < 0) {
            return false;
        }
    }
    return true;
}
```
<br/>

### 字母异位词分组

```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    Map<String, List<String>> map = new HashMap<>();
    for (int i = 0; i < strs.length; i++) {
        // 字符串排序
        char[] arr = new char[26];
        for (char ch : strs[i].toCharArray()) {
            arr[ch - 'a']++;
        }
        String key = String.valueOf(arr);
        // map中不存在key，添加空集合
        if (!map.containsKey(key)) {
            map.put(key, new ArrayList<>());
        }
        map.get(key).add(strs[i]);
    }
    return new ArrayList<>(map.values());
}
```
<br/>

### 二叉树的前序遍历
```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    preOrder(root, result);
    return result;
}

public void preOrder(TreeNode node, List<Integer> list) {
    if (node == null) {
        return;
    }
    // 前序遍历：根-左-右
    list.add(node.val);
    preOrder(node.left, list);
    preOrder(node.right, list);
}
```
<br/>

### 二叉树的中序遍历
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    inOrder(root, result);
    return result;
}

public void inOrder(TreeNode node, List<Integer> list) {
    if (node == null) {
        return;
    }
    // 中序遍历：左-根-右
    inOrder(node.left, list);
    list.add(node.val);
    inOrder(node.right, list);
}
```
<br/>

### N叉树的前序遍历
```java
public List<Integer> preorder(Node root) {
    List<Integer> result = new ArrayList<>();
    preOrder(root, result);
    return result;
}
public void preOrder(Node node, List<Integer> list){
    if (node == null) {
        return;
    }
    // N叉树的前序遍历：根-从左到右子节点遍历
    list.add(node.val);
    if (node.children != null) {
        for (Node childNode : node.children) {
            preOrder(childNode, list);
        }
    }
}
```
<br/>

### N叉树的后序遍历
```java
public List<Integer> postorder(Node root) {
    List<Integer> result = new ArrayList<>();
    postOrder(root, result);
    return result;
}

public void postOrder(Node node, List<Integer> list) {
    if (node == null) {
        return;
    }
    // N叉树后序遍历：从左到右子节点遍历-根
    if (node.children != null) {
        for (Node childNode : node.children) {
            postOrder(childNode, list);
        }
    }
    list.add(node.val);
}
```
<br/>

### N叉树的层序遍历
```java
public List<List<Integer>> levelOrder(Node root) {
    if (root == null) {
        return new ArrayList<>();
    }
    // 双端队列，存子节点
    Deque<Node> deque = new LinkedList<>();
    deque.add(root);
    List<List<Integer>> result = new ArrayList<>();
    while(!deque.isEmpty()){
        int size = deque.size();
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            Node node = deque.poll();
            list.add(node.val);
            deque.addAll(node.children);
        }
        result.add(list);
    }
    return result;
}
```
<br/>

### 最小的k个数

```java
/**
 * 暴力
 */
public int[] getLeastNumbers(int[] arr, int k) {
    if(arr == null || k < 1 || arr.length < k){
        return new int[]{};
    }
    int[] result = new int[k];
    Arrays.sort(arr);
    for(int i = 0; i < k; i++){
        result[i] = arr[i];
    }
    return result;
}
```

```java
/**
 * 最小堆
 */
public int[] getLeastNumbers(int[] arr, int k) {
    if(arr == null || k < 1 || arr.length < k){
        return new int[]{};
    }
    PriorityQueue<Integer> queue = new PriorityQueue<>(k, (i1, i2) -> Integer.compare(i2, i1));
    for (int i = 0; i < arr.length; i++) {
        if (queue.size() > k - 1) {
            if (queue.peek() > arr[i]) {
                queue.poll();
                queue.offer(arr[i]);
            }
        } else {
            queue.offer(arr[i]);
        }
    }
    int[] result = new int[k];
    int index = 0;
    for (Integer num : queue) {
        result[index++] = num;
    }
    return result;
}
```
<br/>

### 滑动窗口最大值
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || k < 1 || nums.length < k) return new int[0];
    if (k == 1) return nums;
    // max:移动框中的最大值     count:移动框中的最大值数量
    int max = 0, count = 0;
    int[] result = new int[nums.length - k + 1];
    Deque<Integer> queue = new LinkedList<>();
    for (int i = 0; i < k; i++) {
        queue.offer(nums[i]);
        max = Math.max(max, nums[i]);
        count = max == nums[i] ? ++count : 1;
    }
    result[0] = max;
    for (int i = k; i < nums.length; i++) {
        int removeNum = queue.poll();
        queue.offer(nums[i]);
        max = Math.max(max, nums[i]);
        count = max == nums[i] ? ++count : 1;
        // 添加新元素后，移除元素值为max且框中无重复max时，重新计算框中的max和count
        if (max == removeNum && --count < 1) {
            max = queue.peek();
            for (Integer num : queue) {
                if (max <= num) {
                    count = max == num ? ++count : 1;
                    max = num;
                }
            }
        }
        result[i - k + 1] = max;
    }
    return result;
}
```
<br/>

### 丑数 II
```java
public int nthUglyNumber(int n) {
    int[] ugly = new int[n];
    ugly[0] = 1;
    int index2 = 0, index3 = 0, index5 = 0;
    int factor2 = 2, factor3 = 3, factor5 = 5;
    for (int i = 1; i < n; i++) {
        int min = Math.min(Math.min(factor2, factor3), factor5);
        ugly[i] = min;
        if (min == factor2) {
            factor2 = 2 * ugly[++index2];
        }
        if (min == factor3) {
            factor3 = 3 * ugly[++index3];
        }
        if (min == factor5) {
            factor5 = 5 * ugly[++index5];
        }
    }
    return ugly[n - 1];
}
```
<br/>

### 前 K 个高频元素
```java
public int[] topKFrequent(int[] nums, int k) {
    // key：元素    value：元素出现次数
    Map<Integer, Integer> map = new HashMap<>();
    for (Integer num : nums) {
        map.put(num, map.containsKey(num) ? map.get(num) + 1 : 1);
    }
    // map中前k高频率元素
    PriorityQueue<Integer> heap = new PriorityQueue<>((n1, n2) -> map.get(n1) - map.get(n2));
    for (Integer key : map.keySet()) {
        if (heap.size() < k) {
            heap.offer(key);
        }else if (map.get(heap.peek()) < map.get(key)) {
            heap.poll();
            heap.offer(key);
        }
    }
    // 结果
    int[] result = new int[k];
    int index = k - 1;
    while(!heap.isEmpty()){
        result[index--] = heap.poll();
    }
    return result;
}
```