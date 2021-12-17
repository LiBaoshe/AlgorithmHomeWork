## [1011. 在 D 天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days/)

### 算法：

二分搜索 + 判断

运载能力的最小值是 weights 中的最大值，记为 left，运载能力最大值是 weights 求和的值，记为 right，那么答案必然在区间 [left, right] 中，因此可以通过二分，每次判断中间值 mid 是否可以满足条件，如果满足就取更小的再判断，直到 left 与 right 相遇结束。

时间复杂度：O(nlogn) 二分的时间复杂度为 O(logn), 每次二分都需要 O(n) 的判断。

空间复杂度：O(1)，计算过程中只定义了几个局部变量。

### 实现代码：

```java
class Solution {

    public int shipWithinDays(int[] weights, int days) {
        int left = 0, right = 0;
        for(int weight : weights){ // 二分搜索初始值
            left = Math.max(left, weight);
            right += weight;
        }
        // 二分查找
        while(left < right){
            int mid = (left + right) / 2;
            if(vaildate(weights, days, mid)){
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return right;
    }

    /**
     * 判断船的最大运载重量为 maxWeight 时，传送带上的包裹能否在 days 天内运送完
     */
    private boolean vaildate(int[] weights, int days, int maxWeight){
        int day = 1, curWeight = 0;
        for(int weight : weights){
            if(curWeight + weight <= maxWeight){
                curWeight += weight;
            } else {
                day++;
                curWeight = weight;
            }
        }
        return day <= days;
    }
}
```

