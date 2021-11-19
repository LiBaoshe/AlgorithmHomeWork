#### [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

算法：最大矩形可以看做以某一行为底边的柱状图中最大的矩形，因此可以遍历每一行，求以当前行作为底边的柱状图中最大的矩形面积，记录全局最大值即可。

时间复杂度：遍历每个元素的时间复杂度为O(mn)，每一行单调栈的时间复杂度为O(n)，整体时间复杂度还是O(mn)。

空间复杂度：记录当前行的高度数组 rowHeight 空间复杂度为O(n)，每一行单调栈的空间复杂度为O(n)，整体空间复杂度还是O(n)

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0){
            return 0;
        }
        // 定义当前行的柱状图高度(数组最后增加一位哨兵，方便单调栈计算)
        int[] rowHeights = new int[matrix[0].length + 1];
        int ans = 0;
        for(int i = 0; i < matrix.length; i++){
            // 计算当前行柱子高度
            for(int j = 0; j < matrix[i].length; j++){
                if(i == 0){ 
                    // 第一行柱子高度等于第一行值
                    rowHeights[j] = matrix[i][j] - '0';
                } else { 
                    // 如果当前值为零，则柱子高度为 0 否则为上一行的高度加一。
                    rowHeights[j] = matrix[i][j] == '0' ? 0 : rowHeights[j] + 1;
                }
            }
            // 计算以当前行为低的柱状图最大面具
            ans = Math.max(ans, largestRectangleArea(rowHeights));
        }
        return ans;
    }

    private Deque<Rect> stack = new ArrayDeque<>();

    private int largestRectangleArea(int[] heights){
        stack.clear();
        int ans = 0;
        for(int i = 0; i < heights.length; i++){
            int accumulatedWidth = 0;
            while(!stack.isEmpty() && stack.peek().height >= heights[i]){
                Rect rect = stack.pop();
                accumulatedWidth += rect.width;
                ans = Math.max(ans, rect.height * accumulatedWidth);
            }
            stack.push(new Rect(accumulatedWidth + 1, heights[i]));
        }
        return ans;
    }

    private class Rect{
        int width;
        int height;
        public Rect(int width, int height){
            this.width = width;
            this.height = height;
        }
    }
}
```

