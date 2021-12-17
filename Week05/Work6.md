## [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

### 方法一：两次二分查找

#### 算法：

第一次二分查找第一列元素，确定目标值可能在哪一行，即第一列中最后一个不大于目标值的行 row；

第二次二分查找第 row 行元素，返回结果。

时间复杂度：O(m) + O(n) = O(mn)

空间复杂度：O(1)

#### 实现代码：

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int left = -1, right = matrix.length - 1;
        // 第一次二分查找，确定目标值可能在哪一行
        while(left < right){
            int mid = (left + right + 1) / 2;
            if(matrix[mid][0] <= target){
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        if(right < 0){
            return false;
        }
        int row = right;
        left = 0;
        right = matrix[0].length - 1;
        // 第二次二分查找，在第 row 行查找目标值
        while(left <= right){
            int mid = (left + right) / 2;
            if(matrix[row][mid] == target){
                return true;
            } else if (matrix[row][mid] < target){
                left = mid + 1;
            }  else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```

### 方法二：一次二分查找

#### 算法：

二维矩阵可以用对应的一维矩阵表示，m * n 的二维矩阵下标 (i, j) 与一维矩阵的下标 y 的映射关系为：

- 二维 → 一维：y = i * n + j
- 一维 → 二维：i = y / n，j = y % n

对二维矩阵的一维表示进行一次二分查找返回结果。

时间复杂度：O(mn)

空间复杂度：O(1)

#### 实现代码：

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int left = 0, right = m * n - 1;
        // 对于 matrix[i][j]，对应的一维映射是 y = i * n + j
        // 则 i = y / n，j = y % n
        while(left <= right){
            int mid = (left + right) / 2;
            int i = mid / n, j = mid % n;
            if(matrix[i][j] == target){
                return true;
            } else if(matrix[i][j] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```

