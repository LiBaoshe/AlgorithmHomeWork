#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

算法：定义保护节点pre，同时遍历两个链表，每次取较小的连接到pre链表上，最后把不为空的链表直接连接到pre链表上。

时间复杂度：O(n)

空间复杂度：O(1)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode pre = new ListNode();
        ListNode cur = pre;
        while (l1 != null && l2 != null){
            if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        if(l1 != null){
            cur.next = l1;
        }
        if(l2 != null){
            cur.next = l2;
        }
        return pre.next;
    }
}
```

