### <a href="https://leetcode-cn.com/problems/number-of-1-bits/">位1的个数<a/>
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        // time:O(num(1))   space:O(1)
        while (n != 0) {
            count++;
            n &= n - 1;
        }
        return count;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/power-of-two/">2的幂<a/>
```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        // time:O(1)    space:O(1)
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/reverse-bits/">颠倒二进制位<a/>
```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ans = 0;
        // time:O(1)    space:O(1)
        for (int i = 0; i < 32; i++) {
            ans = (ans << 1) + (n & 1);
            n >>= 1;
        }
        return ans;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/n-queens/">N皇后<a/>
```java
/**
 * hash表记录 "列状态" 、 "主对角线状态" 、 "副对角线状态"
 */
class Solution {
    List<List<String>> result = new ArrayList<>();
    Set<Integer> cols = new HashSet<>();
    Set<Integer> pies = new HashSet<>();
    Set<Integer> nas = new HashSet<>();
    Deque<Integer> stack = new LinkedList<>();
    public List<List<String>> solveNQueens(int n) {
        // time:O(N!)   space:O(N)
        dfs(n, 0);
        return result;
    }
    private void dfs(int n, int row) {
        if (row == n) {
            List<String> board = convertToBoard(n);
            result.add(board);
            return;
        }
        // 查找row行n列是否可以放皇后
        for (int i = 0; i < n; i++) {
            if (cols.contains(i) || pies.contains(row + i) || nas.contains(row - i)) continue;
            stack.push(i);
            // 列           撇                  捺
            cols.add(i); pies.add(row + i); nas.add(row - i);
            dfs(n, row + 1);
            // 状态还原
            cols.remove(i); pies.remove(row + i); nas.remove(row - i);
            stack.pop();
        }
    }
    private List<String> convertToBoard(int n) {
        List<String> board = new ArrayList<>();
        for (Integer num : stack) {
            StringBuilder builder = new StringBuilder();
            for (int i = 0; i < n; i++) {
                builder.append(".");
            }
            builder.replace(num, num + 1, "Q");
            board.add(builder.toString());
        }
        return board;
    }
}

/**
 * 数组记录 "列状态" 、 "主对角线状态" 、 "副对角线状态"
 */
class Solution {
    char[][] chars;
    boolean[] cols, pies, nas;
    List<List<String>> result;

    public List<List<String>> solveNQueens(int n) {
        result = new ArrayList<>();
        if (n <= 0) return result;
        chars = new char[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(chars[i], '.');
        }
        // 列                       撇                              捺
        cols = new boolean[n]; pies = new boolean[2 * n - 1]; nas = new boolean[2 * n - 1];
        // time:O(N!)       space:O(N ^ 2)
        dfs(n, 0);
        return result;
    }
    private void dfs(int n, int row) {
        if (row == n) {
            List<String> board = convertToBoard(n);
            result.add(board);
            return;
        }
        for (int i = 0; i < n; i++) {
            if (cols[i] || pies[row + i] || nas[row - i + n - 1]) continue;
            chars[row][i] = 'Q';
            cols[i] = pies[row + i] = nas[row - i + n - 1] = true;
            dfs(n, row + 1);
            // 状态还原
            cols[i] = pies[row + i] = nas[row - i + n - 1] = false;
            chars[row][i] = '.';
        }
    }
    private List<String> convertToBoard(int n) {
        List<String> board = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            board.add(new String(chars[i]));
        }
        return board;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/counting-bits/">比特位计数<a/>
```java
class Solution {
    public int[] countBits(int num) {
        int[] ans = new int[num + 1];
        ans[0] = 0;
        // time:O(n)        space:O(1)
        for (int i = 1; i < ans.length; i++) {
            int lowestBit = i & 1;
            if (lowestBit == 1) {
                ans[i] = ans[i - 1] + 1;
            } else {
                int temp = i;
                while (temp != 0) {
                    ans[i]++;
                    temp &= temp - 1;
                }
            }
        }
        return ans;
    }
}

/**
 * 优化代码
 */
class Solution {
    public int[] countBits(int num) {
        int[] ans = new int[num + 1];
        // time:O(n)        space:O(1)
        for (int i = 0; i < ans.length; i++) {
            ans[i] = ans[i >> 1] + (i & 1);
        }
        return ans;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/n-queens-ii/">N皇后 II<a/>
```java
class Solution {
    int count = 0;
    boolean[] cols, pies, nas;
    public int totalNQueens(int n) {
        if (n < 1) return 0;
        cols = new boolean[n];
        pies = new boolean[2 * n - 1];
        nas = new boolean[2 * n - 1];
        // time:O(N!)       space:O(N)
        dfs(n, 0);
        return count;
    }
    private void dfs(int n, int row) {
        if (row == n) {
            count++;
            return;
        }
        for (int i = 0; i < n; i++) {
            if (cols[i] || pies[row + i] || nas[row - i + n - 1]) continue;
            // 列       撇              捺
            cols[i] = pies[row + i] = nas[row - i + n - 1] = true;
            dfs(n, row + 1);
            // 状态还原
            cols[i] = pies[row + i] = nas[row - i + n - 1] = false;
        }
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/relative-sort-array/">数组的相对排序<a/>
```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int[] count = new int[1001];
        // 计数
        for (int num1 : arr1) {
            count[num1]++;
        }
        int index = 0;
        // 处理arr2中的数
        for (int num2 : arr2) {
            while (count[num2] > 0) {
                arr1[index++] = num2;
                count[num2]--;
            }
        }
        // 处理剩余的数
        for (int i = 0; i < count.length; i++) {
            while (count[i] > 0) {
                arr1[index++] = i;
                count[i]--;
            }
        }
        return arr1;
    }
}
```
<br/>