#### [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

算法：

中序、后序遍历的概念：

- 中序遍历：左，根，右

- 后序遍历：左，右，根

递归算法，从中序、后序遍历的特点可以看出，每次从后序遍历中取出根节点，再从中序遍历中找到根节点的位置，将数组分成的左、右两部分表示根节点的左右子树，递归这个过程即可完成二叉树的构造。对每次查找中序数组中对应的位置，可以使用 map 存储数组值和数组下标的映射关系来优化查询。

时间复杂度：O(n)

空间复杂度：O(n)

实现代码：

```java
class Solution {

    int postIdx; // 记录后序遍历数组访问下标
    int[] postorder; // 后序遍历数组
    int[] inorder; // 中序遍历数组
    // idxMap 存储中序数组中数值对应的下标，方便查询
    Map<Integer, Integer> idxMap = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // 初始化数据
        this.postorder = postorder;
        this.inorder = inorder;
        postIdx = postorder.length - 1;
        for (int i = 0; i < inorder.length; i++) {
            idxMap.put(inorder[i], i);
        }
        // 调用递归返回结果
        return helper(0, inorder.length - 1);
    }

    // 递归构造二叉树
    private TreeNode helper(int left, int right) {
        if(left > right){
            return null;
        }
        int rootVal = postorder[postIdx--]; // 取出根节点
        TreeNode root = new TreeNode(rootVal);
        int index = idxMap.get(rootVal);
        root.right = helper(index + 1, right); // 构造左子树
        root.left = helper(left, index - 1); // 构造右子树
        return root;
    }
}
```

