# [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

### 算法：

并查集：遍历二维数组，遇到字符 1 就与 4 个相邻方向上的 1 合并到并查集中，并查集的数量就是岛屿数量。并查集数量统计方法：所有1的数量减去合并成功的次数。

时间复杂度：O(nlogn)，压缩路径后是 O(nα(n))

空间复杂度：O(n)

### 实现代码：

```java
// 并查集
class Solution {

    private final int[] dx = {0, 1, 0, -1};
    private final int[] dy = {-1, 0, 1, 0};
    private int[] fa;
    private int m, n, size, ans;

    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        size = m * n;
        initSet();
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == '0'){
                    continue;
                }
                ans++;
                int x = num(i, j);
                for(int k = 0; k < 4; k++){
                    int ni = i + dx[k], nj = j + dy[k];
                    if(isValid(ni, nj) && grid[ni][nj] == '1'){
                        unionSet(x, num(ni, nj));
                    }
                }
            }
        }
        return ans;
    }
    // 并查集初始化
    private void initSet(){
        fa = new int[size];
        for(int i = 1; i < size; i++){
            fa[i] = i;
        }
    }
    // 并查集合并
    private void unionSet(int x, int y){
        x = find(x);
        y = find(y);
        if(x != y){
            fa[x] = y;
            ans--;
        }
    }
    // 并查集查找
    private int find(int x){
        if(x == fa[x]){
            return x;
        }
        return fa[x] = find(fa[x]);
    }
    // 计算二维坐标对应的一维坐标
    private int num(int i, int j){
        return i * n + j;
    }
    // 判断是否越界
    private boolean isValid(int i, int j){
        return i >= 0 && i < m && j >= 0 && j < n; 
    }
}
```

