# [136. 邻值查找](https://www.acwing.com/problem/content/138/)

尝试用语言内置的有序集合库，或写一棵平衡树，来解决

### 实现代码：

```java
import java.util.Scanner;
import java.util.Arrays;
import java.util.Comparator;

class Node{
    int val;
    int idx;
    Node next;
    Node pre;
    public Node(int val){this.val = val;}
    public Node(int val, int idx){ this.val = val; this.idx = idx;}
}

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] a = new int[n + 1];
        // rk 的含义：rank[i]表示排第i名的是谁（是哪个下标）？
        // Java 中用Integer定义，方便使用Arrays.sort()时传入比较器参数
        Integer[] rk = new Integer[n + 1];
        for(int i = 1; i <= n; i++){
            a[i] = sc.nextInt();
            rk[i] = i;
        }
        Arrays.sort(rk, 1, n + 1, Comparator.comparingInt(rki -> a[rki]));
        // 保护节点
        Node head = new Node(a[rk[1]] - (int) 1e9); // 比最小的数再小1e9 如果是 Integer.MIN_VALUE，计算减法时会溢出
        Node tail = new Node(a[rk[n]] + (int) 1e9); // 比最大的数再大1e9
        head.next = tail;
        tail.pre = head;
        // 定义数组pos快速访问链表
        Node[] pos = new Node[n + 1];
        // 将数组中的数字按排序加入链表
        for(int i = 1; i <= n; i++){
            pos[rk[i]] = addNode(tail.pre, a[rk[i]], rk[i]);
        }
        // 定义结果集，从后往前计算结果
        int[] ans = new int[n + 1];
        for(int i = n; i > 1; i--){
            Node curr = pos[i];
            if(a[i] - curr.pre.val <= curr.next.val - a[i]){
                ans[i] = curr.pre.idx;
            } else {
                ans[i] = curr.next.idx;
            }
            deleteNode(curr);
        }
        for(int i = 2; i <= n; i++){
            System.out.println(Math.abs(a[ans[i]] - a[i]) + " " + ans[i]);
        }
    }
    
    // 双链表插入模板，在node后面插入新节点
    static Node addNode(Node node, int val, int idx){
        Node newNode = new Node(val, idx);
        newNode.next = node.next;
        node.next.pre = newNode;
        newNode.pre = node;
        node.next = newNode;
        return newNode;
    }
    
    // 双链表删除模板
    static void deleteNode(Node node){
        node.pre.next = node.next;
        node.next.pre = node.pre;
        node = node.next = node.pre = null;
    }
}
```

