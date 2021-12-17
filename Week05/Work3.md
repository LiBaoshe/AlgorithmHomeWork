## [875. 爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)

### 算法：

二分查找，最小速度是1，最大速度是 piles 数组中的最大值，通过二分查找每次取中间值判断是否可以吃完香蕉。

时间复杂度：O(nlogn)，二分的时间复杂度为O(logn)，每次二分判断的时间复杂度为 O(n)，整体为O(nlogn)。

空间复杂度：O(1)

### 实现代码：

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        // 初始化二分边界 left, right
        int left = 1, right = Integer.MIN_VALUE;
        for(int pile : piles){
            right = Math.max(right, pile);
        }
        // 二分搜索最小速度 k
        while(left < right){
            int mid = (left + right) / 2;
            if(validate(piles, h, mid)){
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return right;
    }

    /**
     * 验证速度为 k 时能否在 h 小时内吃完香蕉 piles
     */
    private boolean validate(int[] piles, int h, int k){
        int cost = 0;
        for(int pile : piles){
            cost += pile / k;
            if(pile % k != 0){
                cost++;
            }
        }
        return cost <= h;
    }
}
```

