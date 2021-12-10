#### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

#### 算法

BST 的中序遍历（即 左 - 根 - 右）是升序序列，那么安照 右 - 根 - 左 的序列访问就是降序序列，题意可以转化为降序序列时，把前面的数值累加到当前数值上，所以可以定义一个全局变量记录累加和，递归按照 右 - 根 - 左 的序列边访问边累加。

#### 实现代码一：递归遍历

```java
class Solution {

    private int sum;

    public TreeNode convertBST(TreeNode root) {
        sum = 0; // 初始化
        dfs(root); // 递归计算
        return root;
    }

    /**
     * 递归访问 右 - 根 - 左，累加数值
     */
    private void dfs(TreeNode root){
        if(root == null){
            return;
        }
        // 访问右子树
        dfs(root.right); 
        // 访问跟节点
        root.val += sum;
        sum = root.val;
        // 访问左子树
        dfs(root.left);
    }
}
```

时间复杂度：O(n) 需要访问 n 个节点。

空间复杂度：O(logn) 递归栈的空间消耗，平均是 O(logn)，最坏是O(n)。

#### 代码实现二：Morris 遍历

```java
class Solution {

    private int sum;

    public TreeNode convertBST(TreeNode root) {
        TreeNode curr = root;
        sum = 0;
        while(curr != null){
            // 右子树为空，访问当前节点后访问左子树
            if(curr.right == null){ 
                sum += curr.val;
                curr.val = sum;
                curr = curr.left;
            } else  {
                // 右子树不为空，找到降序排列的前驱节点，
                TreeNode next = curr.right;
                while(next.left != null && next.left != curr){
                    next = next.left;
                }
                // 将前驱节点的左子树连接到当前节点，继续向右访问
                if(next.left == null){
                    next.left = curr;
                    curr = curr.right;
                } else { 
                    // 左子树已经访问过，说明又子树访问完毕，可以访问当前节点
                    next.left = null; // 恢复前驱节点的引用
                    sum += curr.val;
                    curr.val = sum;
                    curr = curr.left; // 访问左子树
                }
            }
        }
        return root;
    }
}
```

时间复杂度：O(n) 需要访问 n 个节点，没有左子树的节点只被访问一次，有左子树的节点被访问两次。

空间复杂度：O(1) ，只操作已经存在的指针（树的空闲指针），因此只需要常数的额外空间。
