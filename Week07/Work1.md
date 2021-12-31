# [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

### 算法：

要求：完全平方数看作物品，体积为 n，价值为 1，用背包 DP 的思想解题

状态表达式：f[i] 表示最少需要多少个数的平方来表示整数 i。

时间复杂度：O(n · 根号 n)

空间复杂度：O(n)

### 实现代码：

```java
class Solution {
    public int numSquares(int n) {
        int[] f = new int[n + 1];
        int min = Integer.MAX_VALUE;
        for(int i = 1; i <= n; i++){
            min = Integer.MAX_VALUE;
            for(int j = 1; j * j <= i; j++){
                min = Math.min(min, f[i - j * j]);
            }
            f[i] = min + 1;
        }
        return f[n];
    }
}
```

