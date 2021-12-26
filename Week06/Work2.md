## [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

### 算法：

动态规划

将每一行的左端对齐（因为输入的数据 triangle 每一行都是从 0 开始的）：

```xml
[2]
[3,4]
[6,5,7]
[4,1,8,3]
```

第一行只有一个元素，之后的每一行都比前一行多一个元素，动态规划的初始状态就是第一个元素。

从第二行开始，每行有三种情况：

- 最左边元素：只与上一行第一个关联，最小路径和 = 当前值 + 上一行的第一个路径和
- 中间元素：中间的第 j 个数与上一行的 j 和 j - 1 关联，最小路径和 = 当前值 + min(上一行的第 j 个路径和, 上一行的第 j - 1 个路径和) 
- 最右边元素：只与上一行最后一个关联，最小路径和 = 当前值 + 上一行的最后一个路径和

时间复杂度：O(n^2^) ，每个元素都要遍历一遍。

空间复杂度：O(n) ，每次计算只保存了上过一层的结果和当前层的结果。

### 实现代码：

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        // 记录每一层的路径和
        List<Integer> pathSum = new ArrayList<>();
        // 初始路径和，第一层
        pathSum.add(triangle.get(0).get(0));
        // 从第二层开始计算
        for(int i = 1; i <  triangle.size(); i++){
            // 记录当前层的路径和
            List<Integer> curPathSum = new ArrayList<>();
            List<Integer> level = triangle.get(i);
            int levelSize = level.size();
            // 本行第一个，只与上一行第一个关联
            curPathSum.add(pathSum.get(0) + level.get(0));
            for(int j = 1; j < levelSize - 1; j++){
                // 本行中间的第 j 个数与上一行的 j 和 j - 1 关联
                int sum = Math.min(pathSum.get(j), pathSum.get(j - 1)) + level.get(j);
                curPathSum.add(sum);
            }
            // 本行最后一个，只与上一行最后一个关联
            curPathSum.add(pathSum.get(levelSize - 2) + level.get(levelSize - 1));
            pathSum = curPathSum;
        }
        // 取出最小值返回
        int ans = Integer.MAX_VALUE;
        for(int sum : pathSum){
            ans = Math.min(ans, sum);
        }
        return ans;
    }
}
```

