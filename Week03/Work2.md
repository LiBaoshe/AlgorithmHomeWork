#### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

递归回溯算法，先对数组排序方便去重，对相同的数字，只需要按排序的顺序出现的一个全排列即可。
例如：数组 nums = [A,A,B] 中，我们只需要【第一个A，第二个A，B】这一个序列，而不需要【第二个A，第一个A，B】这样的序列，所以当后边的数与前边的数相等时（nums[i] == nums[i - 1]），如果前边的数没选择进来（!vis[i - 1]），就说明相同的序列已经出现过了，因此可以减枝了。

时间复杂度：O(n×n!)

空间复杂度：O(n)

实现代码：

```java
class Solution {
    
    private boolean[] vis; // 访问标记
    private List<List<Integer>> res; // 记录结果集
    private List<Integer> path; // 当前访问路径

    public List<List<Integer>> permuteUnique(int[] nums) {
        vis = new boolean[nums.length];
        res = new LinkedList<>();
        path = new LinkedList<>();
        Arrays.sort(nums);
        dfs2(nums, 0);
        return res;
    }

    // 递归搜索
    private void dfs2(int[] nums, int k){
        if(k >= nums.length){
            res.add(new LinkedList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            // 针对相同的两个数只需要按顺序出现的一个全排列即可
            if(vis[i] || (i > 0 && nums[i] == nums[i - 1] && !vis[i - 1])){
                continue;
            }
            path.add(nums[i]);
            vis[i] = true;
            dfs2(nums, k + 1);
            vis[i] = false;
            path.remove(path.size() - 1);
        }
    }
}
```

