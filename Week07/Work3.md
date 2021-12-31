# [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

### 算法一：

要求：用动态规划解题，并与之前的贪心解法做对比

状态数组 f[i] 表示跳到第 i 个位置的最小跳跃次数，则对于 j < i，如果可以从 j 跳跃到 i，则有 f[i] = min(f[i]，f[j] + 1)。

时间复杂度：O(n^2^)

空间复杂度：O(n)

### 实现代码：

```java
class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        int f[] = new int[n];
        for(int i = 1; i < n; i++){
            f[i] = Integer.MAX_VALUE;
            for(int j = 0; j < i; j++){
                // 如果从 j 出发可以到达 i，取最小跳跃次数
                if(j + nums[j] >= i){
                    f[i] = Math.min(f[i], f[j] + 1);
                }
            }
        }
        return f[n - 1];
    }
}
```

### 算法二：

贪心算法，遍历数组，记录当前可以跳跃到的最远距离，记为边界，当每次遍历到达边界时，更新边界并将跳跃次数增加 1。

时间复杂度：O(n)

空间复杂度：O(1)

### 实现代码：

```java
class Solution {
    public int jump(int[] nums) {
        int count = 0;
        int end = 0;
        int maxIndex = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            maxIndex = Math.max(maxIndex, i + nums[i]);
            if (i == end){
                end = maxIndex;
                count++;
            }
        }
        return count;
    }
}
```

