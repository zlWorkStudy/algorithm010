### <a href="https://leetcode-cn.com/problems/implement-trie-prefix-tree/">实现 Trie (前缀树)</a>
```java
class Trie {
    private boolean isEnd;
    private Trie[] next;
    /** Initialize your data structure here. */
    public Trie() {
        isEnd = false;
        next = new Trie[26];
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        if (word == null || word.length() == 0) return;
        Trie cur = this;
        char[] words = word.toCharArray();
        for (int i = 0; i < words.length; i++) {
            int n = words[i] - 'a';
            if (cur.next[n] == null) cur.next[n] = new Trie();
            cur = cur.next[n];
        }
        cur.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        Trie node = searchPrefix(prefix);
        return node != null;
    }

    private Trie searchPrefix(String word) {
        if (word == null) return null;
        Trie cur = this;
        char[] words = word.toCharArray();
        for (int i = 0; i < words.length; i++) {
            cur = cur.next[words[i] - 'a'];
            if (cur == null) return null;
        }
        return cur;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/friend-circles/">朋友圈</a>
```java
class Solution {
    private int find(int[] parent, int p) {
        while (p != parent[p]) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }

    private void union(int[] parent, int p, int q) {
        int rootP = find(parent, p);
        int rootQ = find(parent, q);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
    }

    public int findCircleNum(int[][] M) {
        int[] parent = new int[M.length];
        for (int i = 0; i < M.length; i++) {
            parent[i] = i;
        }
        for (int i = 0; i < M.length - 1; i++) {
            for (int j = i + 1; j < M.length; j++) {
                if (M[i][j] == 1) union(parent, i, j);
            }
        }
        int count = 0;
        for (int i = 0; i < parent.length; i++) {
            if (parent[i] == i) count++;
        }
        return count;
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/number-of-islands/">岛屿数量</a>
```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid[0].length == 0) return 0;
        int row = grid.length, col = grid[0].length, count = 0;
        // O(M * N)
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }

    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || j < 0 || i >=  grid.length || j >= grid[0].length || grid[i][j] == '0') return;
        grid[i][j] = '0';
        int[][] dirs = {{0, 1}, {1,0}, {-1, 0}, {0, -1}};
        for (int[] dir : dirs) {
            dfs(grid, i + dir[0], j + dir[1]);
        }
    }
}
```
<br/>

### <a href="https://leetcode-cn.com/problems/surrounded-regions/">被围绕的区域</a>
```java
class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0) return;
        int row = board.length, col = board[0].length;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                boolean isBoard = i == 0 || j == 0 || i == row - 1 || j == col - 1;
                if (isBoard && board[i][j] == 'O') {
                    dfs(board, i, j);
                }
            }
        }
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
                if (board[i][j] == '#') {
                    board[i][j] = 'O';
                }
            }
        }
    }

    private void dfs(char[][] board, int i, int j) {
        if (i < 0 || j < 0 || i >= board.length || j >= board[0].length || board[i][j] == 'X' || board[i][j] == '#') return;
        board[i][j] = '#';
        int[][] dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        for (int[] dir : dirs) {
            dfs(board, i + dir[0], j + dir[1]);
        }
    }
}
```
<br/>
