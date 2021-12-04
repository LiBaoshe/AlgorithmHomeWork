#### [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

要求：用分治实现，不要用堆

算法：二分合并，每次将 lists 分成两半，各自递归合并。递归出口：只有一个链表时直接返回，两个链表时合并返回，其它情况均二分递归。

时间复杂度：O(kn×logk)

空间复杂度：O(logk) 

实现代码：

```java
class Solution {

    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists, 0, lists.length - 1);
    }

    // 递归合并 K 个升序链表
    private ListNode merge(ListNode[] lists, int l, int r){
        if(l > r){
            return null;
        }
        if(l == r){
            return lists[l];
        }
        if(r - l == 1){
            return merge(lists[l], lists[r]);
        }
        int mid = (l + r) / 2;
        ListNode leftNode = merge(lists, l, mid);
        ListNode rightNode = merge(lists, mid + 1, r);
        return merge(leftNode, rightNode);
    }

    // 递归合并两个升序链表
    private ListNode merge(ListNode head1, ListNode head2){
        ListNode protect = new ListNode();
        ListNode cur = protect;
        while(head1 != null && head2 != null){
            if(head1.val < head2.val){
                cur.next = head1;
                head1 = head1.next;
            } else {
                cur.next = head2;
                head2 = head2.next;
            }
            cur = cur.next;
        }
        cur.next = head1 == null ? head2 : head1;
        return protect.next;
    }
}
```

