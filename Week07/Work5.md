# [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

### 算法：

DFS + 动态规划 + HashMap

对每个节点 node 计算一次经过该节点的最大路径，然后求全局 最大路径即可。

用 HashMap f(node, maxGain)，保存每个节点的最大贡献值 maxGain，定义全局最大路径 ans，DFS 遍历二叉树计算每个节点的最大路径，更新 ans。

时间复杂度：O(n)，DFS 中每个节点访了问一遍。

空间复杂度：O(n)，状态记录 f(node, maxGain) 和 递归栈都是 O(n) 的空间复杂度。

### 实现代码：

```java
class Solution {
    // 记录节点的最大贡献值
    private Map<TreeNode, Integer> f;
    // 记录返回结果最大路径
    private int ans;
    public int maxPathSum(TreeNode root) {
        // 初始化
        f = new HashMap<>();
        ans = Integer.MIN_VALUE;
        f.put(null, 0);
        // 递归计算
        dfs(root);
        return  ans;
    }
    // 二叉树的递归遍历
    private void dfs(TreeNode root){
        if(root == null){
            return;
        }
        dfs(root.left);
        dfs(root.right);
        // 计算最大贡献值
        int maxGain = Math.max(f.get(root.left), f.get(root.right)) + root.val;
        maxGain = Math.max(maxGain, root.val);
        // 计算经过当前节点的最大路径
        int curMaxPath = Math.max(root.val, f.get(root.left) + f.get(root.right) + root.val);
        curMaxPath = Math.max(curMaxPath, f.get(root.left) + root.val);
        curMaxPath = Math.max(curMaxPath, f.get(root.right) + root.val);
        // 更新答案
        ans = Math.max(ans, curMaxPath);
        // 记录最大贡献值
        f.put(root, maxGain);
    }
}
```

