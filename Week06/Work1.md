## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

### 算法：

动态规划

用 f(i) 表示爬到第 i 台阶的方案数，最后一次只有两种可能：爬了一个台阶或爬了两个台阶，状态转移方程可以写为：f(i)  =  f(i - 1) + f(i - 2) 。初始值 f(1) = 1，f(2) = 2，当 n > 2 时，根据状态转移方程递推计算方案数。由于当前值只与前两个结果有关，所以定义两个变量 x, y 记录前两次的结果。

时间复杂度：O(n)

空间复杂度：O(1)

### 实现代码：

```java
class Solution {
    public int climbStairs(int n) {
        if(n <= 2){
            return n;
        }
        int x = 1, y = 2, res = 0;
        for (int i = 3; i <= n; i++) {
            res = x + y;
            x = y;
            y = res;
        }
        return res;
    }
}
```



