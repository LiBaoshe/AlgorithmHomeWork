#### [560. 和为 K 的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

算法：前缀和 + 哈希表

时间复杂度：O(n)

空间复杂度：O(n)

```java
class Solution {
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

