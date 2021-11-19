#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)

`class Solution {`

  `public int[] plusOne(int[] digits) {`

​    `int i = digits.length - 1;`

​    `for (; i >= 0; i--) {`

​      `if(digits[i] < 9){`

​        `digits[i]++;`

​        `break;`

​      `} else {`

​        `digits[i] = 0;`

​      `}`

​    `}`

​    `if(i < 0){`

​      `digits = new int[digits.length + 1];`

​      `digits[0] = 1;`

​    `}`

​    `return digits;`

  `}`

`}`



算法：倒序遍历数组，如果遇到小于9的数则直接加一返回，如果是9就变为0，继续判断前一位，如果全是9，则数组长度加一，第一位为1，其余位置全为0。

时间复杂度：需要遍历数组，为O(n)

空间复杂度：O(1)