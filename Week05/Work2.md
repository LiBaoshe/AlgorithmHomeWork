## [911. 在线选举](https://leetcode-cn.com/problems/online-election/)

### 算法：

哈希统计初始化 + 二分查找返回。

按时间遍历，用哈希表 voteCount 统计候选人的票数，统计过程中记录当前票数最高的人 curTop，保存到数组 tops[i] 中。返回时 times 中二分查找最后一个不大于 t 的位置 i，返回 tops[i]。

时间复杂度：哈希统计初始化的时间复杂度为O(n)，二分查找返回的时间复杂度为O(logn)。

空间复杂度：数组存储了每个时间对应的候选人，时间复杂度为O(n)

### 实现代码：

```java
class TopVotedCandidate {

    private int[] times; // 记录时刻
    private int[] tops; // 时刻 times[i] 对应的领先的候选人编号是 tops[i]

    public TopVotedCandidate(int[] persons, int[] times) {
        this.times = times;
        tops = new int[times.length];
        // 统计票数
        Map<Integer, Integer> voteCount = new HashMap<>();
        int curTop = -1; // 当前领先的候选人的编号
        for(int i = 0; i < times.length; i++){
            int p = persons[i];
            voteCount.put(p, voteCount.getOrDefault(p, 0) + 1);
            if(voteCount.get(p) >= voteCount.getOrDefault(curTop, 0)){
                curTop = p;
            }
            tops[i] = curTop;
        }
    }
    
    public int q(int t) {
        // times 中二分查找最后一个不大于 t 的位置 i，返回 tops[i]
        int left = 0, right = times.length - 1;
        while(left < right){
            int mid = (left + right + 1) / 2;
            if(times[mid] <= t){
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return tops[right];
    }
}
```

