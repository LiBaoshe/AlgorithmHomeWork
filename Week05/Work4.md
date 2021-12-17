## [327. 区间和的个数](https://leetcode-cn.com/problems/count-of-range-sum/)

### 算法：

前缀和 + 归并排序

计算出前缀和 preSum，则问题等价与 后边与前边的前缀和之差位于范围 [lower, upper]，利用归并排序中当前位置的左边和右边是有序的两个数组的特点，在归并排序过程中，每次合并两个有序数组之前计算可以优化时间复杂度。

时间复杂度：O(nlogn)，归并排序的时间复杂度为O(nlogn)，统计函数 calc 的时间复杂度为O(n)

空间复杂度：O(n)，合并数组需要额外O(n)的空间

### 实现代码：

```java
class Solution {

    private int count; // 统计区间和个数
    private int lower;
    private int upper;

    public int countRangeSum(int[] nums, int lower, int upper) {
        count = 0;
        this.lower = lower;
        this.upper = upper;
        // 计算前缀和
        long[] preSum = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++){
            preSum[i + 1] = preSum[i] + nums[i];
        }
        // 归并排序过程中统计
        mergeSort(preSum, 0, preSum.length - 1);
        return count;
    }

    // 有序数组 [l, mid] 和 [mid + 1, r] 中统计 前缀和之差在 [lower, upper] 中的个数，
    // 右边选一个 preSum, 左边选一个 preSum，右边preSum - 左边preSum 在 [lower, upper] 中
    private void calc(long[] preSum, int l, int mid, int r){
        int i = mid + 1, j = mid + 1;
        for(int k = l; k <= mid; k++){
            while(i <= r && preSum[i] - preSum[k] < lower){
                i++;
            }
            while(j <= r && preSum[j] - preSum[k] <= upper){
                j++;
            }
            count += (j - i);
        }
    }

    // 归并排序算法
    private void mergeSort(long[] arr, int l, int r){
        if(l >= r){
            return;
        }
        int mid = (l + r) / 2;
        mergeSort(arr, l, mid);
        mergeSort(arr, mid + 1, r);
        calc(arr, l, mid, r); // 归并之前计算统计
        merge(arr, l, mid, r);
    }

    private void merge(long[] a, int l, int mid, int r){
        long[] temp = new long[r - l + 1];
        int i = l, j = mid + 1;
        // 合并两个有序数组 [l, mid] 和 [mind + 1, r]
        for(int k = 0; k < temp.length; k++){
            if(j > r || (i <= mid && a[i] <= a[j])){
                temp[k] = a[i++];
            } else {
                temp[k] = a[j++];
            }
        }
        // 考回原数组
        for(int k = 0; k < temp.length; k++){
            a[l + k] = temp[k];
        }
    }
}
```

