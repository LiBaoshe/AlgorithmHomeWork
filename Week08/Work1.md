# [684. 冗余连接](https://leetcode-cn.com/problems/redundant-connection/)

### 算法：

并查集：遍历边集，对每条边的两个顶点，如果在同一个集合，则出现环，返回这条边。如果在不同集合中，则合并两个顶点。

时间复杂度：O(nlogn)，压缩路径后是 O(nα(n))

空间复杂度：O(n)

### 实现代码：

```java
class Solution {

    private int[] fa;

    public int[] findRedundantConnection(int[][] edges) {
        // 初始化并查集
        fa = new int[edges.length + 1];
        for(int i = 0; i < fa.length; i++){
            fa[i] = i;
        }

        for(int[] edge : edges){
            int x = edge[0], y = edge[1];
            if(find(x) == find(y)){
                return edge;
            }
            unionSet(x, y);
        }

        return new int[]{};
    }

    // 和并 x, y
    private void unionSet(int x, int y){
        x = find(x);
        y = find(y);
        if(x != y){
            fa[x] = y;
        }
    }

    // 查找
    private int find(int x){
        if(x == fa[x]){
            return x;
        }
        // 路径压缩并返回
        return fa[x] = find(fa[x]);
    }
}
```

