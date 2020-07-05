### <a href="https://leetcode-cn.com/problems/binary-tree-level-order-traversal/">二叉树的层序遍历</a>
```java
/**
 * bfs (广度优先)
 */
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    // 存每层元素
    Deque<TreeNode> deque = new LinkedList<>();
    deque.offer(root);
    while (!deque.isEmpty()) {
        int size = deque.size();
        List<Integer> levelList = new ArrayList<>();
        result.add(levelList);
        // 处理树的当前层元素
        for (int i = 0; i < size; i++) {
            TreeNode node = deque.pop();
            levelList.add(node.val);
            // 向队列中添加下一层的元素
            if (node.left != null) deque.offer(node.left);
            if (node.right != null) deque.offer(node.right);
        }
    }
    return result;
}

/**
 * dfs (深度优先)
 */
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> levelOrder(TreeNode root) {
    dfs(root, 0);
    return result;
}
private void dfs(TreeNode node, int level) {
    if (node == null) return;
    if (level == result.size()) {
        // 当前深度的元素还未存过元素，添加新集合
        List<Integer> list = new ArrayList<>();
        list.add(node.val);
        result.add(list);
    } else {
        result.get(level).add(node.val);
    }
    // 左右子节点继续下探
    dfs(node.left, level + 1);
    dfs(node.right, level + 1);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/">在每个树行中找最大值</a>
```java
/**
 * bfs (广度优先)
 */
public List<Integer> largestValues(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    Deque<TreeNode> deque = new LinkedList<>();
    deque.offer(root);
    while (!deque.isEmpty()) {
        int size = deque.size(), levelMax = Integer.MIN_VALUE;
        for (int i = 0; i < size; i++) {
            TreeNode node = deque.pop();
            if (levelMax < node.val) levelMax = node.val;
            // 向队列中添加下一层元素
            if (node.left != null) deque.offer(node.left);
            if (node.right != null) deque.offer(node.right);
        }
        result.add(levelMax);
    }
    return result;
}

/**
 * dfs (深度优先)
 */
List<Integer> result = new ArrayList<>();
public List<Integer> largestValues(TreeNode root) {
    dfs(root, 0);
    return result;
}
private void dfs(TreeNode node, int level) {
    if (node == null) return;
    if (level == result.size()) {
        result.add(node.val);
    } else if (result.get(level) < node.val) {
        result.set(level, node.val);
    }
    // 左右子节点下探
    dfs(node.left, level + 1);
    dfs(node.right, level + 1);
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/lemonade-change/description/">柠檬水找零</a>
```java
public boolean lemonadeChange(int[] bills) {
    /*
     * 排队找零
     *   1. 5美元, five++
     *   2. 10美元, ten++, five--
     *   3. 20美元, (ten-- and five--) or (five - 3)
     */
    int five = 0, ten = 0;
    for (int bill : bills) {
        if (bill == 5) {
            five++;
        } else if (bill == 10) {
            ten++;
            five--;
        } else if (ten > 0) {
            ten--;
            five--;
        } else {
            five -=3;
        }
        if (five < 0 || ten < 0) return false;
    }
    return true;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/">买卖股票的最佳时机 II</a>
```java
public int maxProfit(int[] prices) {
    /*
     * 当天股票价格如果小于后一天的股票价格
     * 则买入当天并在后一天卖出
     */
    if (prices == null || prices.length < 2) return 0;
    int count = 0;
    for (int i = 1; i < prices.length; i++) {
        int temp = prices[i] - prices[i - 1];
        if (temp > 0) count += temp;
    }
    return count;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/assign-cookies/">分发饼干</a>
```java
public int findContentChildren(int[] g, int[] s) {
    /*
     * 对g,s进行排序
     * 饼干尺寸大的满足胃口大的孩子, 分发饼干数量就是满足数量
     */
    if (g == null || s == null) return 0;
    Arrays.sort(g);
    Arrays.sort(s);
    int gIndex = g.length - 1, sIndex = s.length - 1;
    while (gIndex >= 0 && sIndex >= 0) {
        if (g[gIndex--] <= s[sIndex]) {
            sIndex--;
        }
    }
    return s.length - sIndex - 1;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/walking-robot-simulation/">模拟行走机器人</a>
```java
public int robotSim(int[] commands, int[][] obstacles) {
    /*
     * 1. 机器人出发方向(转向指令确定机器人行走方向): 
     *      y轴正方向(0, 1), x轴正方向(1,0),y轴负方向(0,-1),x轴负方向(-1,0)
     * 2. 机器人从(0, 0)向y轴正方向(北方)出发
     * 3. 判断当前移动路线(一格一格移动判断)中是否存在障碍物
     *      存在: 停在障碍物前一格, 继续下一条命令
     *      不存在: 在x轴或y轴上行走
     * 4. 当前移动命令结束后计算距离, 与之前的最大距离值作比较
     */
    // 存障碍点
    Set<Long> set = new HashSet<>();
    for (int[] obs : obstacles) {
        // 计算原理? 位运算学习后需要明白, 存String类型耗时是位运算的5倍左右
        long ox = (long) obs[0] + 30000;
        long oy = (long) obs[1] + 30000;
        set.add((ox << 16) + oy);
    }
    // 方向对应的x、y轴的前进值
    int[][] dirs = {{0, 1}, {1, 0}, {0, -1}, {-1 ,0}};
    int d = 0, x = 0, y = 0, result = 0;
    for (int cmd : commands) {
        if (cmd == -1) {
            d = (d + 1) % 4;
        } else if (cmd == -2) {
            d = (d + 3) % 4;
        } else {
            while (cmd-- > 0 && !set.contains((((long) x + dirs[d][0] + 30000) << 16) + ((long) y + dirs[d][1] + 30000))) {
                x += dirs[d][0];
                y += dirs[d][1];
            }
            result = Math.max(result, x * x + y * y);
        }
    }
    return result;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/jump-game/">跳跃游戏</a>
```java
public boolean canJump(int[] nums) {
    /*
     * 从后往前计算
     *      需要到达位置下标targetIndex, targetIndex - curIndex <= nums[curIndex]时
     *      targetIndex = curIndex, curIndex--, 否则 curIndex--
     * targetIndex == 0时, 可以到达
     */
    int targetIndex = nums.length - 1, curIndex = nums.length - 2;
    while (curIndex >= 0) {
        if (targetIndex - curIndex <= nums[curIndex])
            targetIndex = curIndex;
        curIndex--;
    }
    return targetIndex == 0;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/sqrtx/">x 的平方根</a>
```java
public int mySqrt(int x) {
    // 二分法   x的平方根整数部分 < x / 2 (1除外)
    long left = 0,  right = x / 2 + 1;
    while (left < right) {
        // 取右中位数，防止死循环，例: x = 9, left = 3, right = 4时死循环
        long mid = (left + right + 1) / 2;
        long square = mid * mid;
        if (square > x) {
            right = mid - 1;
        } else {
            left = mid;
        }
    }
    return (int)left;
}

public int mySqrt(int x) {
    // 牛顿迭代法
    long curNum = x;
    while (curNum * curNum > x) {
        curNum = (curNum + x / curNum) / 2;
    }
    return (int)curNum;
}

public int mySqrt(int x) {
    // 数学公式换底计算
    if ( x == 0) return 0;
    int ans = (int) Math.exp(0.5 * Math.log(x));
    return (long)(ans + 1) * (ans + 1) <= x ? ans + 1 : ans;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/valid-perfect-square/">有效的完全平方数</a>
```java
public boolean isPerfectSquare(int num) {
    /*
     * num < 2, 返回true
     * 左边界left = 2, 右边界 right = num / 2
     * left <= right时
     *      mid = (left + right) >> 1, square = mid * mid, square与square比较
     *          square == num返回true
     *          square > num,  right = mid - 1
     *          square < num,  left = mid + 1
     */
    if (num < 2) return true;
    long left = 2, right = num >> 1;
    while (left <= right) {
        long mid = (left + right) >> 1;
        long square = mid * mid;
        if (square == num) {
            return true;
        }else if (square > num) {
            right = mid - 1;
        } else {
            left = mid + 1; 
        }
    }
    return false;
}

public boolean isPerfectSquare(int num) {
    // 牛顿迭代公式
    long curNum = num;
    while (curNum * curNum > num) {
        curNum = (curNum + num / curNum) / 2;
    }
    return curNum * curNum == num;
}

public boolean isPerfectSquare(int num) {
    // 数学公式换底
    int ans = (int)Math.exp(0.5 * Math.log(num));
    ans = (long)(ans + 1) * (ans + 1) <= num ? ans + 1 : ans;
    return ans * ans == num;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/search-in-rotated-sorted-array/">搜索旋转排序数组</a>
```java
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1, index = 0;
    while (left <= right) {
        index = (left + right + 1) / 2;
        int curNum = nums[index];
        if (curNum == target) {
            return index;
        } else if (curNum > nums[left]) {
            // index左边数据有序
            if (target >= nums[left] && target < curNum) {
                right = index - 1;
            } else {
                left = index + 1;
            }
        } else {
            // index 右边数据有序
            if (target <= nums[right] && target > curNum) {
                left = index + 1;
            } else {
                right = index - 1;
            }
        }
    }
    return -1;
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/search-a-2d-matrix/">搜索二维矩阵</a>
```java
public boolean searchMatrix(int[][] matrix, int target) {
    if ( matrix.length == 0) return false;
    int col = matrix.length, row = matrix[0].length;
    int left = 0, right = col * row - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (matrix[mid / row][mid % row] == target) {
            return true;
        } else if (matrix[mid / row][mid % row] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
}
```
<br/>