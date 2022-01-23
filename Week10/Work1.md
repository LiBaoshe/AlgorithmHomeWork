# [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

尝试用语言内置的有序集合库，或写一棵平衡树，来解决

### 算法：

优先队列懒惰删除法。

时间复杂度：O(nlogn)

空间复杂度：O(n)

### 实现代码：

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        PriorityQueue<Pair<Integer, Integer>> q = new PriorityQueue<>((p1, p2) -> p2.getKey() - p1.getKey());
        int[] ans = new int[nums.length - k + 1];
        for(int i = 0; i < nums.length; i++){
            q.add(new Pair(nums[i], i));
            if(i >= k - 1){
                // 如果越界就删除
                while(q.peek().getValue() <= i - k){
                    q.remove();
                }
                ans[i - k + 1] = q.peek().getKey();
            }
        }
        return ans;
    }
}
```

