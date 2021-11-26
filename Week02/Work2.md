#### [697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

算法：结果与数组的度对应的数字（可能有多个数字）和数字的开始位置和结束位置有关，所以遍历数组计算数组的度的时候顺便把数字的开始位置和结束位置也记录下来（这里定义了一个class Node记录），使用list记录数组的度对应的数字，最后遍历list里的数字，根据map中记录的数字开始结束位置计算出最短连续子数组的长度。

时间复杂度：O(n)

空间复杂度：O(n)

实现代码：

```java
class Solution {
    private class Node{
        int d; // 记录数字的度
        int start; // 记录数字的开始位置
        int end; // 记录数字的结束位置
        public Node(int d, int start){
            this.d = d;
            this.start = this.end = start;
        }
    }
    public int findShortestSubArray(int[] nums) {
        Map<Integer, Node> map = new HashMap<>();
        int d = 1; // 记录数组的度
        // 记录数组的度对应的数字
        List<Integer> list = new LinkedList<>(); 
        for(int i = 0; i < nums.length; i++){
            Node node = map.getOrDefault(nums[i], new Node(0, i));
            node.d++;
            node.end = i;
            if(node.d > d){
                d = node.d;
                list.clear();
                list.add(nums[i]);
            } else if(node.d == d){
                list.add(nums[i]);
            }
            map.put(nums[i], node);
        }
        int ans = Integer.MAX_VALUE;
        for(int n : list){
            Node node = map.getOrDefault(n, new Node(0,0));
            ans = Math.min(ans, node.end - node.start + 1);
        }
        return ans;
    }
}
```

