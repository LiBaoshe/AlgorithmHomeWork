## [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

### 算法：

二分搜索，每次中间值与右端值做比较，共有三种情况：

- 中间值 = 右端值  此时无法确定最小值在哪边，右端点左移一位。
- 中间值 < 右端值  此时最小值一定在左边，搜素左边。
- 中间值  > 右端值  此时最小值一定在右边，搜素右边。

时间复杂度：平均O(logn)，最好的情况是没有重复元素，时间复杂度为O(logn)，最坏的情况是全部重复元素，时间复杂度为O(n)。

空间复杂度：O(1)

### 实现代码：

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = (left + right) / 2;
            if(nums[mid] == nums[right]) {
                // 中间值 == 右端值，无法确定最小值在哪边，线性搜素
                right --;
            } else if(nums[mid] < nums[right]) {
                // 中间值 < 右端值，最小值一定在左半部分，包括 mid
                right = mid;
            } else {
                // 中间值 > 右端值，最小值一定在右半部分，不包括 mid
                left = mid + 1;
            }
        }
        return nums[right];
    }
}
```

