# [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

### 算法：

动态规划，定义状态空间 f(i,j) 表示区间 [i, j] 的最长的回文子序列长度，第 i 个字符表示为 ci，第 j 个字符表示为 cj 则

- f(i,i) = 1，单个字符是一个回文序列
- i > j 时，f(i, j) = 0，i < j 时根据 ci 和 cj 计算 f(i, j)
- ci == cj 时，f(i, j) = f(i + 1, j) + 2
- ci != cj 时，f(i, j) = max(f(i + 1, j), f(i, j - 1))

时间复杂度：O(n^2^)

空间复杂度：O(n^2^)

### 实现代码：

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        // 状态空间，f[i][j] 表示区间 [i, j] 的最长的回文子序列长度
        int[][] f = new int[n][n];
        for(int i = n - 1; i >= 0; i--){
            f[i][i] = 1;
            char ci = s.charAt(i);
            for(int j = i + 1; j < n; j++){
                char cj = s.charAt(j);
                if(ci == cj){
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
                }
            }
        }
        return f[0][n - 1];
    }
}
```

