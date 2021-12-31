# [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

### 算法：

贪心算法，遍历数组，记录每次可以跳跃的最远距离 reach，如果 reach 小于当前位置 i，则不能跳到最后。

时间复杂度：O(n)

空间复杂度：O(1)

### 实现代码：

```java
class Solution {
    public boolean canJump(int[] nums) {
        int reach = 0;
        for (int i = 0; i < nums.length; i++) {
            if(i > reach){
                return false;
            }
            reach = Math.max(reach, nums[i] + i);
            if(reach >= nums.length - 1){
                return true;
            }
        }
        return true;
    }
}
```

