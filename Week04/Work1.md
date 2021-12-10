#### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

#### 算法

所有不被包围的 O 都与边界处的 O 连通，所以只要找到所有与边界处的 O 相连的连通区域，剩下的元素都是 X。从边界处的 O 出发，通过 BFS 或 DFS 搜索，将连通的 O 都暂时标记为 V，然后将没有标记过的 O 变成 X ，将标记过的 V 恢复为 O 即可。

时间复杂度：O(m x n)，m, n 分别为行数和列数，每个元素都需要遍历一遍。

空间复杂度：O(m x n)，DFS 中栈的深度开销，BFS 中队列的长度开销都是 m x n 的。

#### 实现代码

**注意：**BFS 搜索的时候，标记的位置入完队列后就要立即标记，不要等到出队列的时候才标记，否则会导致未标记的位置重复进入队列，运行超时。

BFS 与 DFS 只是搜索的时候不同，代码框架是一样的：

```java
class Solution {

    private char[][] board; // 记录输入矩阵
    private int m; // 行数
    private int n; // 列数
    // 定义方向数组
    private int[] dx = {-1, 0, 0, 1};
    private int[] dy = {0, -1, 1, 0};

    public void solve(char[][] board) {
        this.board = board;
        m = board.length;
        n = board[0].length;
        // 搜索左右边界
        for(int i = 0; i < m; i++){
            search(i, 0);
            search(i, n - 1);
        }
        // 搜索上下边界
        for(int j = 0; j < n; j++){
            search(0, j);
            search(m - 1, j);
        }
        // 将没有标记过的 O 改成 X，标记过的 V 恢复成 O
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                } else if(board[i][j] == 'V'){
                    board[i][j] = 'O';
                }
            }
        }
    }

    // 如果是 O 就开始标记连通区域
    private void search(int i, int j){
        if(board[i][j] == 'O'){
            bfs(i, j); // BFS 标记
            // dfs(i, j); // DFS 标记
        }
    }

    /**
     * 通过 DFS 算法标记
     */
    private void dfs(int i, int j){
        if(!valid(i, j)){
            return;
        }
        if(board[i][j] == 'O'){
            board[i][j] = 'V';
            for(int k = 0; k < 4; k++){
                dfs(i + dx[k], j + dy[k]);
            }
        }
    }

    /**
     * 通过 BFS 算法标记
     */
    private void bfs(int i, int j){
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[]{i, j});
        board[i][j] = 'V';  // 入队后就标记
        while(!q.isEmpty()){
            int[] cell = q.remove();
            int x = cell[0];
            int y = cell[1];
            for(int k = 0; k < 4; k++){
                int nx = x + dx[k];
                int ny = y + dy[k];
                if(valid(nx, ny) && board[nx][ny] == 'O'){
                    q.add(new int[]{nx, ny});
                    board[nx][ny] = 'V'; // 入队后就标记
                }
            }
        }
    }

    // 判断数组下标是否越界
    private boolean valid(int i, int j){
        return i >= 0 && i < m && j >= 0 && j < n;
    }
}
```

