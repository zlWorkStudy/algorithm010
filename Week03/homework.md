### <a href="https://leetcode-cn.com/problems/climbing-stairs/">爬楼梯</a>
```java
/**
 * 递归
 */
public int climbStairs(int n) {
    return fibonacci(n, 1, 1);
}

public int fibonacci(int n, int a, int b) {
    return n <= 1 ? b : fibonacci(n - 1, b, a + b);
}

/**
 * 循环累加 f(n) = f(n - 1) + f(n - 2);
 */
public int climbStairs(int n) {
    int first = 1, second = 1;
    for (int i = 1; i < n; i++) {
        int temp = second;
        second += first;
        first = temp;
    }
    return second;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/generate-parentheses/">括号生成</a>
```java
List<String> result = new ArrayList<>();
public List<String> generateParenthesis(int n) {
    recurse(n, n, "");
    return result;
}

public void recurse(int left, int right, String str) {
    if (left == 0 && right == 0) {
        result.add(str);
        return;
    }
    // add "("
    if (left > 0) recurse(left - 1, right, str + "(");
    // add ")"
    if (right > left) recurse(left, right - 1, str + ")");
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/invert-binary-tree/description/">翻转二叉树</a>
```java
public TreeNode invertTree(TreeNode root) {
    recurse(root);
    return root;
}
public void recurse(TreeNode node) {
    if (node == null) return;
    // 翻转左右子节点
    TreeNode left = node.left;
    node.left = node.right;
    node.right = left;
    // 遍历左子树
    recurse(node.left);
    // 遍历右子树
    recurse(node.right);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/validate-binary-search-tree">验证二叉搜索树</a>
```java
public boolean isValidBST(TreeNode root) {
    return recurse(root, null, null);
}

public boolean recurse(TreeNode node, Integer lowwer, Integer upper) {
    if (node == null) return true;
    int val = node.val;
    /* 
     * 根节点的左子树中:
     *    右子节点大于父节点且小于最近节点是父节点左节点的值
     *    左子节点小于父节点
     * 根节点的右子树中:
     *    左子节点小于父节点且大于最近节点是父节点右节点的值
     *    右子节点大于父节点
     */
    if (lowwer != null && val <= lowwer) return false;
    if (upper != null && val >= upper) return false;
    return recurse(node.left, lowwer, val) && recurse(node.right, val, upper);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/">二叉树的最大深度</a>
```java
int max;

public int maxDepth(TreeNode root) {
    recurse(root, 0);
    return max;
}

public void recurse(TreeNode node, int curDepth) {
    if (node == null) {
        max = Math.max(max, curDepth);
        return;
    }
    // left child
    recurse(node.left, curDepth + 1);
    // right child
    recurse(node.right, curDepth + 1);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/">二叉树的最小深度</a>
```java
public int minDepth(TreeNode root) {
    if (root == null) return 0;
    // left child
    int s1 = minDepth(root.left);
    // right child
    int s2 = minDepth(root.right);
    /*
     * 根节点到最小子节点的距离
     * 无左节点时，从右节点中找，反之亦然
     */
    return root.left == null || root.right == null ? s1 + s2 + 1 : Math.min(s1, s2) + 1;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/powx-n/">Pow(x, n)</a>
```java
public double myPow(double x, int n) {
    return n > 0 ? recurse(x, n) : 1 / recurse(x, -n);
}
public double recurse(double x, int n) {
    if (n == 0) return 1;
    double result = recurse(x, n / 2);
    return n % 2 == 0 ? result * result : result * result * x;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/subsets/">子集</a>
```java
/**
 * 前序遍历
 */
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> subsets(int[] nums) {
    result.add(new ArrayList<>());
    recurse(nums, 0, new ArrayList<>());
    return result;
}

public void recurse(int[] nums, int index, List<Integer> subSet) {
    if (index >= nums.length) {
        return;
    }
    subSet = new ArrayList<>(subSet);
    result.add(subSet);
    recurse(nums, index + 1, subSet);
    subSet.add(nums[index]);
    recurse(nums, index + 1, subSet);
}

/**
 * 回溯
 */
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> subsets(int[] nums) {
    result.add(new ArrayList<>());
    recurse(nums, 0, new ArrayList<>());
    return result;
}

public void recurse(int[] nums, int index, List<Integer> subSet) {
    if (index >= nums.length) {
        return;
    }
    subSet.add(nums[index]);
    result.add(new ArrayList<>(subSet));
    recurse(nums, index + 1, subSet);
    subSet.remove(subSet.size() - 1);
    recurse(nums, index + 1, subSet);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/majority-element/">多数元素</a>
```java
public int majorityElement(int[] nums) {
    int flag = nums[0], count = 1;
    for (int i = 1; i < nums.length; i++) {
        if (count == 0) flag = nums[i];
        count += nums[i] == flag ? 1 : -1;
    }
    return flag;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/">电话号码的字母组合</a>
```java
List<String> result = new ArrayList<>();
Map<Character, String[]> map = new HashMap<>();
{
    map.put('2', new String[]{"a", "b", "c"});
    map.put('3', new String[]{"d", "e", "f"});
    map.put('4', new String[]{"g", "h", "i"});
    map.put('5', new String[]{"j", "k", "l"});
    map.put('6', new String[]{"m", "n", "o"});
    map.put('7', new String[]{"p", "q", "r", "s"});
    map.put('8', new String[]{"t", "u", "v"});
    map.put('9', new String[]{"w", "x", "y", "z"});
}

public List<String> letterCombinations(String digits) {
    if (digits == null || digits.length() == 0) return result;
    recurse(digits, 0, "");
    return result;
}

public void recurse(String digits, int index, String str) {
    if (index >= digits.length()) {
        result.add(str);
        return;
    }
    for (String s : map.get(digits.charAt(index))) {
        recurse(digits, index + 1, str + s);
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/">二叉树的最近公共祖先</a>
```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode leftNode = lowestCommonAncestor(root.left, p, q);
    TreeNode rightNode = lowestCommonAncestor(root.right, p, q);
    return leftNode != null && rightNode != null ? root : (leftNode == null ? rightNode : leftNode);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/">从前序与中序遍历序列构造二叉树</a>
```java
Map<Integer, Integer> indexMap = new HashMap<>();
public TreeNode buildTree(int[] preorder, int[] inorder) {
    for (int i = 0; i < inorder.length; i++) {
        indexMap.put(inorder[i], i);
    }
    return recurse(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
}

public TreeNode recurse(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
    if (preStart > preEnd) return null;
    // 获取根节点在inorder中的位置
    int rootVal = preorder[preStart];
    // 创建根节点
    TreeNode root = new TreeNode(rootVal);
    int pIndex = indexMap.get(rootVal);
    // 左子树
    root.left = recurse(preorder, preStart + 1, pIndex - inStart + preStart, inorder, inStart, pIndex - 1);
    // 右子树
    root.right = recurse(preorder, preEnd + pIndex - inEnd + 1, preEnd, inorder, pIndex + 1, inEnd);
    return root;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/combinations/">组合</a>
```java
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> combine(int n, int k) {
    if (n < k) throw new RuntimeException("Incorrect input data.");
    recurse(n, 1, k, new ArrayList<>());
    return result;
}

public void recurse(int n, int cur, int k, List<Integer> list) {
    if (list.size() == k) {
        result.add(list);
        return;
    }
    list.add(cur);
    if (cur <= n) recurse(n, cur + 1, k, new ArrayList<>(list));
    if (n - cur + list.size() - 1 >= k) {
        // 回溯
        list.remove(list.size() - 1);
        recurse(n, cur + 1, k, new ArrayList<>(list));
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/permutations/submissions/">全排列</a>
```java
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> permute(int[] nums) {
    recurse(nums, 0);
    return result;
}

public void recurse(int[] nums, int index) {
    if (index == nums.length - 1) {
        List<Integer> list = new ArrayList<>();
        for (Integer num : nums) {
            list.add(num);
    }
        result.add(list);
        return;
    }
    for (int i = index; i< nums.length; i++) {
        swap(nums, i, index);
        recurse(nums, index + 1);
        swap(nums, i, index);
    }
}

private void swap(int[] nums, int i, int j) {
    // 交换数值
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```