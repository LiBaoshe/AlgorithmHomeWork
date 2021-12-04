#### [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

算法：构造出边数组，进行广度优先搜索（BFS）拓扑排序

时间复杂度：O(n + m)，其中 n 表示节点数，m 表示边数。

空间复杂度：O(n + m)

实现代码：

```java
class Solution {

    private int[] lessens; // 记录课程顺序
    private int lesIdx; // 课程顺序下标
    private List<Integer>[] to; // 出边数组
    private int[] inDeg; // 记录节点的入度

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // 初始化
        lesIdx = 0;
        lessens = new int[numCourses];
        inDeg = new int[numCourses];
        to = new ArrayList[numCourses];
        for(int i = 0; i < numCourses; i++){
            to[i] = new ArrayList<>();
        }
        for(int[] edge : prerequisites){
            int ai = edge[0];
            int bi = edge[1];
            to[bi].add(ai);
            inDeg[ai]++;
        }
        // 广度优先搜（BFS）索拓扑排序
        Queue<Integer> q = new LinkedList<>();
        for(int i = 0; i < numCourses; i++){
            if(inDeg[i] == 0){ // 所有入度为 0 的节点先入队列
                q.add(i);
            }
        }
        while(!q.isEmpty()){
            int x = q.remove();
            lessens[lesIdx++] = x;
            for(int y : to[x]){
                inDeg[y]--;
                if(inDeg[y] == 0){
                    q.add(y);
                }
            }
        }
        // 如果可以修完所有课程则返回结果，否则返回空数组
        return lesIdx == numCourses ? lessens : new int[]{};
    }
}
```

