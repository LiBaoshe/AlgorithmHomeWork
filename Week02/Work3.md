#### [1074. 元素和为目标值的子矩阵数量](https://leetcode-cn.com/problems/number-of-submatrices-that-sum-to-target/)

算法：前缀和 + 哈希表，枚举上边界和下边界，从上边界累加到下边界变成一维数组，问题转换为 和为 K 的子数组。

时间复杂度：O(m<sup>2</sup>·n)

空间复杂度：O(n)

实现代码：

```java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int ans = 0, m = matrix.length, n = matrix[0].length;
        for(int i = 0; i < m; i++){ // 枚举上边界
            int[] nums = new int[n];
            for(int j = i; j < m; j++){ // 枚举下边界
                for(int k = 0; k < n; k++){
                    nums[k] += matrix[j][k]; // 累加每列元素
                }
                ans += subarraySum(nums, target); // 计算每次枚举的结果
            }
        }
        return ans;
    }

    /**
     * 560. 和为 K 的子数组
     */
    public int subarraySum(int[] nums, int k) {
        int len = nums.length;
        int[] s = new int[len + 1];
        Map<Integer, Integer> count = new HashMap<>();
        count.put(s[0], 1);
        int ans = 0;
        for(int i = 1; i <= len; i++){
            s[i] = s[i - 1] + nums[i - 1];
            int x = s[i] - k;
            if(count.containsKey(x)){
                ans += count.get(x);
            }
            count.put(s[i], count.getOrDefault(s[i], 0) + 1);
        }
        return ans;
    }
}
```

