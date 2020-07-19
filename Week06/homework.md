#### <a href="https://leetcode-cn.com/problems/dungeon-game/">地下城游戏</a>
```java
public int calculateMinimumHP(int[][] dungeon) {
    int row = dungeon.length;
    if (row == 0) return 0;
    int col = dungeon[0].length;
    if (col == 0) return 0;
    int[][] dp = new int[row + 1][col + 1];
    // 赋值 O(M * N)
    for (int i = 0; i < row + 1; i++) {
        Arrays.fill(dp[i], Integer.MAX_VALUE);
    }
    dp[row][col - 1] = 1;
    dp[row - 1][col] = 1;
    // O(M * N)
    for (int i = row - 1; i >= 0; i--) {
        for (int j = col - 1; j >= 0; j--) {
            // dp方程 f(i,j) = max(min(f(i,j+1), f(i+1,j)-dungeon(i,j)), 1);
            int min = Math.min(dp[i][j + 1], dp[i + 1][j]);
            dp[i][j] = Math.max(min - dungeon[i][j], 1);
        }
    }
    return dp[0][0];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/unique-paths/">不同路径</a>
```java
public int uniquePaths(int m, int n) {
    /*
     * 分析: 从start到当前网格的走法是只有两种(从上或者从左到达当前网格)
     *   重复性: 到达当前网格的走法 = 上一个网格的走法 + 左一格网格的走法
     *   状态数组: int[][] dp = new int[m][n];
     *   DP 方程: f(i, j) = f(i - 1, j) + f(i, j - 1);
     */
    int[][] dp = new int[m][n];
    // 第一排和第一列只有一种走法能到达，全设为1
    for (int i = 1; i < m; i++) {
        dp[i][0] = 1;
    }
    Arrays.fill(dp[0], 1);
    // 时间:O(M * N)    空间: O(M * N)
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[m - 1][n - 1];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/unique-paths-ii/">不同路径 II</a>
```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int row = obstacleGrid.length, col = obstacleGrid[0].length;
    // 状态数组
    int[][] dp = new int[row][col];
    // dp 第一行赋值，遇到障碍后面位置不用赋值      O(M)
    for (int i = 0; i < col; i++) {
        if (obstacleGrid[0][i] == 1) break;
        dp[0][i] = 1;
    }
    // dp 第一列赋值，遇到障碍后面的位置不用赋值    O(N)
    for (int i = 0; i < row; i++) {
        if (obstacleGrid[i][0] == 1) break;
        dp[i][0] = 1;
    }
    /*
     * 重复问题: 
     *      当前位置有障碍物: dp[i][j] = 0
     *      当前位置无障碍物: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
     * 时间 O(M * N),   空间 O(M * N)
     */
    for (int i = 1; i < row; i++) {
        for (int j = 1; j < col; j++) {
            /*
             * dp 方程: 
             *      f(i, j) = 0                             obstacleGrid[i][j] = 1;
             *      f(i, j) = f(i - 1, j) + f(i, j - 1)     other
             */
            if (obstacleGrid[i][j] == 0)
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[row - 1][col - 1];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/longest-common-subsequence/">最长公共子序列</a>
```java
public int longestCommonSubsequence(String text1, String text2) {
    /*
     * 重复问题:
     *      当前位置两字符串的最大重复子序列长度
     *          1. 上一个位置和左一个位置的最大长度  text1.charAt(i) != text2.charAt(j)
     *          2. 左上角位置长度值 + 1             text1.charAt(i) == text2.charAt(j)
     */
    int row = text1.length(), col = text2.length();
    // 状态数组
    int[][] dp = new int[row + 1][col + 1];
    char[] rowChar = text1.toCharArray();
    char[] colChar = text2.toCharArray();
    // 时间: O(M * N), 空间: O(M * N)
    for (int i = 1; i < row + 1; i++) {
        for (int j = 1; j < col + 1; j++) {
            /*
             *  dp 方程
             *      f(i, j) = f(i - 1, j - 1) + 1             rowChar[i - 1] == colChar[j - 1]
             *      f(i, j) = max(f(i - 1, j), f(i - 1, j))   rowChar[i - 1] != colChar[j - 1]
             */
            if (rowChar[i - 1] == colChar[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[row][col];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/triangle/">三角形最小路径和</a>
```java
public int minimumTotal(List<List<Integer>> triangle) {
    /*
     * 重复问题
     *      当前位置值等于下一层i位置值和i+1位置值得最小值加当前位置值
     * 状态数组
     *      int[] dp = new int[triangle.get(triangle.size() - 1).size()]
     * dp 方程
     *      f(i, j) = min(f(i + 1， j), f(i + 1, j + 1)) + triangle(i, j)
     * time:O(n)    space:O(n)
     */
    int[] dp = new int[triangle.get(triangle.size() - 1).size() + 1];
    for (int i = triangle.size() - 1; i >= 0; i--) {
        List<Integer> current = triangle.get(i);
        for (int j = 0; j < current.size(); j++) {
            dp[j] = Math.min(dp[j], dp[j + 1]) + current.get(j);
        }
    }
    return dp[0];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/maximum-subarray/">最大子序和</a>
```java
public int maxSubArray(int[] nums) {
    /*
     * 重复问题：
     *      当前位置之前连续子数组和sum大于0时，sum += nums[i];
     *      否则，sum = nums[i];
     *      最后sum跟ans比较，取最大值
     * 状态数组
     *      简化为变量
     * dp 方程
     *      f(i) = max(ans, f(i - 1) + nums[i])     f(i - 1) > 0
     *      f(i) = max(ans, nums[i])                f(i - 1) <= 0
     */
    int sum = 0, ans = nums[0];
    for (int i = 0; i < nums.length; i++) {
        if (sum > 0) {
            sum += nums[i];
        } else {
            sum = nums[i];
        }
        ans = Math.max(ans, sum);
    }
    return ans;
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/maximum-product-subarray/">乘积最大子数组</a>
```java
public int maxProduct(int[] nums) {
    /*
     * ans 当前位置最大连续子数组乘积
     * maxPre 当前位置前一个位置的最大连续子数组乘积
     * minPre 当前位置前一个位置的最大连续子数组乘积的绝对值，当前位置为0时重新计算
     *
     * 重复问题
     *      crrentMax = max(maxPre * current, minPre * current, current)
     *      ans = max(ans, currentMax)
     * 状态数组     变量
     * dp 方程
     *      swap(maxPre, minPre)                        nums[i] < 0
     *      f(i) = max(maxPre * nums[i], nums[i])
     */
    int ans = nums[0], maxPre = nums[0], minPre = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] < 0) {
            int temp = maxPre;
            maxPre = minPre;
            minPre = temp;
        }
        maxPre = Math.max(maxPre * nums[i], nums[i]);
        minPre = Math.min(minPre * nums[i], nums[i]);
        ans = Math.max(ans, maxPre);
    }
    return ans;
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/coin-change/">零钱兑换</a>
```java
public int coinChange(int[] coins, int amount) {
    // 重复问题：当前金额, 循环coins数组, 获取当前金额所需最小硬币个数
    int max = amount + 1;
    // 状态数组
    int[] dp = new int[max];
    Arrays.fill(dp, max);
    dp[0] = 0;
    // time: O(S * N)   space: O(S)
    for (int i = 1; i < max; i++) {
        for (int j = 0; j < coins.length; j++) {
            // dp方程: f(i) = min(f(i), f(i - coins[j]) + 1)
            if (coins[j] <= i) {
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/house-robber/">打家劫舍</a>
```java
public int rob(int[] nums) {
    /*
     * 重复问题: 
     *  偷 i 房子的金额最大值取一下情况的最大值
     *      偷 i - 2房子金额加 i 房子的金额
     *      偷 i - 1房子金额且不偷i房子金额
     */
    // 状态数组
    int[] dp = new int[nums.length + 2];
    // time: O(n)   space: O(n)
    for (int i = 0; i < nums.length; i++) {
        // dp方程   f(i) = max(f(i - 1), f(i - 2) + nums[i])
        dp[i + 2] = Math.max(dp[i] + nums[i], dp[i + 1]);
    }
    return dp[nums.length + 1];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/house-robber-ii/">打家劫舍 II</a>
```java
public int rob(int[] nums) {
    /*
     * 重复问题: 
     *  分两种情况
     *      偷nums[0], 不偷nums[nums.length - 1]
     *      偷nums[nums.length - 1], 不偷nums[0]
     *  偷 i 房子的金额最大值取一下情况的最大值
     *      偷 i - 2房子金额加 i 房子的金额
     *      偷 i - 1房子金额且不偷i房子金额
     */
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    // 状态数组
    int[] dp = new int[nums.length + 2];
    // time: O(n)   space: O(n)
    for (int i = 1; i < nums.length; i++) {
        // dp方程   f(i) = max(f(i - 1), f(i - 2) + nums[i])
        dp[i + 2] = Math.max(dp[i] + nums[i], dp[i + 1]);
    }
    for (int i = 0; i < nums.length - 1; i++) {
        dp[i + 2] = Math.max(dp[i] + nums[i], dp[i + 1]);
    }
    return Math.max(dp[nums.length], dp[nums.length + 1]);
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/">卖股票的最佳时机</a>
```java
public int maxProfit(int[] prices) {
    /*
     * 重复问题: 
     *      第i天前买入股票, 计算第i天的股票价格卖出, 获利最大值
     *      与第i - 1天的获利最大值比较, 保存最大值
     */
    if (prices == null || prices.length == 0) return 0;
    // 状态数组
    int[] dp = new int[prices.length];
    int lowwer = prices[0];
    // time: O(n)   space: O(n)
    for (int i = 1; i < prices.length; i++) {
        // f(i) = max(prices[i] - lowwer, f(i - 1))
        dp[i] = Math.max(prices[i] - lowwer, dp[i - 1]);
        if (prices[i] < lowwer) {
            lowwer = prices[i];
        }
    }
    return dp[prices.length - 1];
}

/**
 * 状态数组优化
 */
public int maxProfit(int[] prices) {
    /*
     * 重复问题: 
     *      第i天前买入股票, 计算第i天的股票价格卖出, 获利最大值
     *      与第i - 1天的获利最大值比较, 保存最大值
     */
    if (prices == null || prices.length == 0) return 0;
    int max = 0, lowwer = prices[0];
    // time: O(n)   space: O(1)
    for (int i = 1; i < prices.length; i++) {
        // f(i) = max(prices[i] - lowwer, f(i - 1))
        max = Math.max(prices[i] - lowwer, max);
        if (prices[i] < lowwer) {
            lowwer = prices[i];
        }
    }
    return max;
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/minimum-path-sum/">最小路径和</a>
```java
public int minPathSum(int[][] grid) {
    /*
     * 重复问题
     *      当前位置到右下角的最小路径 = min(下一格路径, 右一格路径) + grid[i][j]
     */
    int row = grid.length;
    if (row == 0) return 0;
    int col = grid[0].length;
    // 状态数组
    int[][] dp = new int[row + 1][col + 1];
    for (int i = 0; i < row; i++) {
        dp[i][col] = Integer.MAX_VALUE;
    }
    Arrays.fill(dp[row], Integer.MAX_VALUE);
    dp[row - 1][col] = 0;
    dp[row][col - 1] = 0;
    // time: O(M * N)   space: O(M * N)
    for (int i = row - 1; i >= 0; i--) {
        for (int j = col - 1; j >= 0; j--) {
            // dp方程: f(i, j) = min(f(i, j + 1), f(i + 1, j)) + grid[i][j]
            dp[i][j] = Math.min(dp[i][j + 1], dp[i + 1][j]) + grid[i][j];
        }
    }
    return dp[0][0];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/decode-ways/">解码方法</a>
```java
public int numDecodings(String s) {
    /*
     * 重复问题:
     *      当前位置解码种数 = 前一位之前的解码种数 + 前两位之前的解码种数
     */
    int len = s.length();
    if (len == 0) return 0;
    // 状态数组
    int[] dp = new int[len];
    char[] charArr = s.toCharArray();
    if (charArr[0] == '0') return 0;
    dp[0] = 1;
    // time: O(n)   space: O(n)
    for (int i = 1; i < len; i++) {
        /*
         * dp方程
         *      f(i) = 0                        s(i) == 0
         *      f(i) = f(i - 1) + f(i - 2)      10 < s(i - 1, i) < 27 && s(i) != '0'
         *      f(i) = f(i - 1)                 !(10 < s(i - 1, i) < 27) && s(i) != '0'
         */
        if (charArr[i] != '0') dp[i] = dp[i - 1];
        int num = 10 * (charArr[i - 1] - '0') + charArr[i] - '0';
        if (num > 9 && num < 27) {
            if (i == 1) dp[i]++;
            else dp[i] += dp[i - 2];
        }
    }
    return dp[len - 1];
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/maximal-square/">最大正方形</a>
```java
public int maximalSquare(char[][] matrix) {
    int row = matrix.length;
    if (row == 0) return 0;
    int col = matrix[0].length, maxSide = 0;
    // 状态数组
    int[][] dp = new int[row + 1][col + 1];
    // time: o(M * N)   space: O(M * N)
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (matrix[i][j] == '1') {
                /*
                 * dp方程: 
                 *  f(i, j) = min(min(f(i - 1, j), f(i, j - 1)), f(i - 1, j - 1)) + 1    matrix[i][j] == '1'
                 *  f(i, j) = 0                                                          others
                 */
                dp[i + 1][j + 1] = Math.min(Math.min(dp[i + 1][j], dp[i][j + 1]), dp[i][j]) + 1;
                maxSide = Math.max(maxSide, dp[i + 1][j + 1]);
            }
        }
    }
    return maxSide * maxSide;
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/palindromic-substrings/">回文子串</a>
```java
public int countSubstrings(String s) {
    if (s == null || s.length() == 0) return 0;
    int len = s.length();
    char[] charArr = s.toCharArray();
    // 状态数组
    boolean[][] dp = new boolean[len][len];
    int result = len;
    for (int i = 0; i < len; i++) {
        dp[i][i] = true;
    }
    // time: O(n ^ 2)       space: O(n ^ 2)
    for (int i = len - 1; i >= 0; i--) {
        for (int j = i + 1; j < len; j++) {
            // dp方程: f(i, j) = (j - i == 1) ? true : f(i + 1, j - 1)
            if (charArr[i] == charArr[j]) {
                dp[i][j] = j - i == 1 ? true : dp[i + 1][j - 1];
                if (dp[i][j]) result++;
            }
        }
    }
    return result;
}
```

<br/>

#### <a href="https://leetcode-cn.com/problems/task-scheduler/">任务调度器</a>
```java
public int leastInterval(char[] tasks, int n) {
    int[] nums = new int[26];
    for (int i = 0; i < tasks.length; i++) {
        nums[tasks[i] - 'A']++;
    }
    Arrays.sort(nums);
    int maxVal = nums[25] - 1, idelSlot = maxVal * n;
    for (int i = 24; i >= 0 && nums[i] > 0; i--) {
        idelSlot -= Math.min(nums[i], maxVal);
    }
    return idelSlot > 0 ? idelSlot + tasks.length : tasks.length;
}
```

<br/>