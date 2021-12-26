## [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

### 算法：

动态规划

定义 dp[i] 表示以 nums[i] 结尾的最长上升子序列的长度，cnt[i] 表示以 nums[i] 结尾的最长上升子序列的个数，最长上升子序列的长度计为 maxLen，那么答案为所有满足 dp[i] = maxLen 的 i 所对应的 cnt[i] 之和。

时间复杂度：O(n^2^)

空间复杂度：O(n)

### 实现代码：

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        // 记录以位置 i 结尾的最长递增数
        int[] dp = new int[nums.length]; 
        // 记录以位置 i 结尾的最长递增子序列个数
        int[] cnt = new int[nums.length]; 
        // 初始状态都为 1
        dp[0] = 1;
        cnt[0] = 1;
        int ans = 1, maxLen = 1;
        // 动态规划计算
        for(int i = 1; i < nums.length; i++){
            dp[i] = cnt[i] = 1;
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]){
                    if(dp[j] + 1 > dp[i]){
                        // 发现更长的递增子序列，更新结果
                        dp[i] = dp[j] + 1;
                        cnt[i] = cnt[j];
                    } else if(dp[j] + 1 == dp[i]){
                        // 长度相等的递增子序列累加个数
                        cnt[i] += cnt[j];
                    }
                }
            }
            if(dp[i] > maxLen){
                // 最大长度改变了，更新计数
                maxLen = dp[i];
                ans = cnt[i];
            } else if(dp[i] == maxLen){
                // 最大长度没改，累加结果
                ans += cnt[i];
            }
        }
        return ans;
    }
}
```

