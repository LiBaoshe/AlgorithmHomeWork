#### [641. 设计循环双端队列](https://leetcode-cn.com/problems/design-circular-deque/)

算法：使用双向链表数据结构实现。

时间复杂度：每次都是在队头或队尾操作，时间复杂度为O(1)

空间复杂度：需要保存 n 个节点，空间复杂度为O(n)

```java
class MyCircularDeque {

    private int capacity; // 定义容量
    private int size; // 记录当前容量

    private class Node{ // 定义链表节点
        int val;
        Node pre;
        Node next;
        public Node(){}
        public Node(int val){this.val = val;}
        public Node(int val, Node pre, Node next){this.val = val; this.pre = pre; this.next = next;}
    }

    private Node head; // 定义头结点
    private Node tail; // 定义尾节点

    // 添加节点，添加到 node 的后边
    private void addNode(Node node, int val){
        Node newNode = new Node(val, node, node.next);
        node.next.pre = newNode;
        node.next = newNode;
    }

    private void delNode(Node node){
        node.next.pre = node.pre;
        node.pre.next = node.next;
        node = node.pre = node.next = null;
    }

    public MyCircularDeque(int k) {
        head = new Node();
        tail = new Node();
        head.next = tail;
        tail.pre = head;
        this.capacity = k;
    }
    
    public boolean insertFront(int value) {
        if(isFull()){
            return false;
        }
        addNode(head, value);
        size++;
        return true;
    }
    
    public boolean insertLast(int value) {
        if(isFull()){
            return false;
        }
        addNode(tail.pre, value);
        size++;
        return true;
    }
    
    public boolean deleteFront() {
        if(isEmpty()){
            return false;
        }
        delNode(head.next);
        size--;
        return true;
    }
    
    public boolean deleteLast() {
        if(isEmpty()){
            return false;
        }
        delNode(tail.pre);
        size--;
        return true;
    }
    
    public int getFront() {
        if(isEmpty()){
            return -1;
        }
        return head.next.val;
    }
    
    public int getRear() {
        if(isEmpty()){
            return -1;
        }
        return tail.pre.val;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
    
    public boolean isFull() {
        return size == capacity;
    }
}
```

