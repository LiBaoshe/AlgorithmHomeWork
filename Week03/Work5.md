#### [685. 冗余连接 II](https://leetcode-cn.com/problems/redundant-connection-ii/)

算法：根据节点入度和深度优先搜索环求解。

- 如果存在入度为 2 的节点，则能删除的边一定在入度为2的节点上，删除一条与该节点入度有关的边，如果删除后满足树的条件则返回这条边。先计算出每个点的入度，再将可能删除的边按顺序加入栈中，利用栈的后进先出优先删除最后出现的边。
- 如果不存在入度为2的节点，则一定有环，用深度优先搜索找到环上的路径，选取环中最后出现的边返回。
- 找环算法：DFS 过程中，用0表示尚未访问的节点，1表示正在访问的节点，2表示已经访问完的节点，如果深度搜索过程中遇到了正在访问（访问标记为1）的节点，则表示有环。

时间复杂度：O(nlogn)

空间复杂度：O(n)

实现代码：

```java
class Solution {

    // 访问记录，0=尚未访问，1=正在访问，2=已完毕
    // 递归遇到 1 号状态的点，说明有环。
    private int[] visit;
    private List<Integer>[] to; // 出边数组
    private int n; // 节点个数
    private List<Pair<Integer, Integer>> path; // 当前访问路径
    private List<Pair<Integer, Integer>> cyclePath; // 记录环路径
    private int[] inDeg; // 节点入度
    private Integer[] rk; // rk[i] 表示 节点入度的下标，按入度降序访问节点
    private int maxInDeg; // 节点最大入度

    public int[] findRedundantDirectedConnection(int[][] edges) {
        n = 0;
        // 查找 n
        for(int[] edge : edges){
            n = Math.max(n, Math.max(edge[0], edge[1]));
        }
        // 初始化
        visit = new int[n + 1];
        to = new ArrayList[n + 1];
        inDeg = new int[n + 1];
        rk = new Integer[n + 1];
        for(int i = 1; i <= n; i++){
            to[i] = new ArrayList<>();
            rk[i] = i;
        }
        for(int[] edge : edges){
            int ui = edge[0];
            int vi = edge[1];
            to[ui].add(vi);
            inDeg[vi]++;
            maxInDeg = Math.max(maxInDeg, inDeg[vi]);
        }
        Arrays.sort(rk, 1, n + 1, (rki, rkj) -> inDeg[rkj] - inDeg[rki]);
        // 如果存在入度为 2 的节点，则删除的边一定是该节点入度对应的边
        if(maxInDeg > 1){
            // 用栈记录可能删除的边，后进先出，可以优先删除最后出现的边
            Deque<Pair<Integer, Integer>> stack = new ArrayDeque<>();
            for(int[] edge : edges){
                int ui = edge[0];
                int vi = edge[1];
                if(inDeg[vi] > 1){
                    stack.push(new Pair(ui, vi));
                }
            }
            while(!stack.isEmpty()){
                Pair<Integer, Integer> pair = stack.pop();
                int ui = pair.getKey();
                int vi = pair.getValue();
                inDeg[vi]--; // 如果删除这条边，则 vi 入度减一
                if(isTree()){ // 判断删除后是否是树
                    return new int[]{ui, vi};
                }
                inDeg[vi]++; // 如果删除后不是答案，则恢复入度
            }
        }

        findCycle(); // 找环

        Map<Integer, Pair<Integer, Integer>> map = new HashMap<>();
        for(Pair<Integer, Integer> pair : cyclePath){
            map.put(pair.getKey(), pair);
        }

        for(int i = edges.length - 1; i >= 0; i--){
            int ui = edges[i][0];
            if(map.containsKey(ui)){
                int vi = map.get(ui).getValue();
                if(vi == edges[i][1] && inDeg[vi] == maxInDeg ){
                    return edges[i];
                }
            }
        }

        return new int[]{};

    }

    // 判断是否是树
    // 1. 入度为零的节点只有一个
    // 2. 其余节点入度只能是1
    // 3. 不存在环
    private boolean isTree(){
        int count = 0; // 统计入度为 0 的节点
        for(int i = 1; i <= n; i++){
            if(inDeg[i] > 1){
                return false;
            } else if(inDeg[i] == 0){
                count++;
            }
            if(count > 1){
                return false;
            }
        }
        // 找环
        findCycle();
        return count == 1 && cyclePath.isEmpty();
    }

    // 深度优先搜索找环
    private void dfs(int x, int fa){
        visit[x] = 1; // 正在访问
        for(int y : to[x]){
            // 遇到访问过的父节点，回退
            if(y == fa && visit[fa] == 2){
                if(!path.isEmpty()){
                    path.remove(path.size() - 1);
                }
                continue;
            }
            path.add(new Pair(x, y)); // 记录访问路径
            if(visit[y] == 0){ // 尚未访问过的点
                dfs(y, x);
            } else if (visit[y] == 1){ // 遇到正在访问的点，则有环
                cyclePath = new ArrayList<>(path);
            }
        }
        visit[x] = 2; // 访问完成
    }

    // 找环
    private void findCycle(){
        // 初始化找环条件
        path = new ArrayList<>();
        cyclePath = new ArrayList<>();
        for(int i = 1; i <= n; i++){
            visit[i] = 0;
        }
        // 查找下一个开始访问的点
        int x = findNext();
        while(x > 0){
            dfs(x, 0); // 递归找环
            if(cyclePath.size() > 0){
                break;
            }
            x = findNext();
        }
    }

    // 查找下一个可能存在环的节点，优先返回入度大的节点
    private int findNext(){
        for(int i = 1; i <= n; i++){
            if(visit[rk[i]] == 0 ){
                return rk[i];
            }
        }
        return 0;
    }
}
```

