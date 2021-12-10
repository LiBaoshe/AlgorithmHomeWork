#### [355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)

#### 算法

哈希表 + 链表，哈希表存储用户信息，key 表示 userId，value 存储关注的用户 id 集合与推文信息，推文中同时保存时间顺序用于合并时排序。getNewsFeed 就是将自己的推特链表与所有关注的推特链表合并取前 recentMax 个，这个操作与 [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/) 基本等同。

时间复杂度：

- postTweet 的时间复杂度为 O(1)，链表的插入删除为O(1)的复杂度。
- getNewsFeed 的时间复杂度为 O(recentMax∗num)，其中 recentMax = 10， num 为用户关注的人加上自己的数量和。
- follow 和 unfollow 的时间复杂度都是 O(1)，哈希表插入删除为  O(1) 的时间复杂度。

空间复杂度：O(recentMax∗tot)，其中 recentMax = 10，tot 为推特用户总数

#### 实现代码

```java
class Twitter {
    
    // 推文结构
    private class Tweet {
        int tweetId; // 推文编号
        int tweetSno; // 推文序号，按时间递增
        public Tweet(int tweetId, int tweetSno){
            this.tweetId = tweetId;
            this.tweetSno = tweetSno;
        }
    }
    private int sno; // 全局序号，按时间递增

    // 用户结构
    private class User {
        // 哈希表存储关注人的 ID
        Set<Integer> followee = new HashSet<>();
        // 链表存储 tweet
        LinkedList<Tweet> tweet = new LinkedList<>();
    }

    // 哈希表存储用户
    private Map<Integer, User> userMap;
    // 检索推文的上限
    private int recentMax;

    public Twitter() {
        sno = 0;
        recentMax = 10;
        userMap = new HashMap<>();
    }
    
    public void postTweet(int userId, int tweetId) {
        if(!userMap.containsKey(userId)){
            userMap.put(userId, new User());
        }
        User user = userMap.get(userId);
        // 达到限制，剔除链表末尾元素
        if(user.tweet.size() >= recentMax){
            user.tweet.remove(recentMax - 1);
        }
        user.tweet.addFirst(new Tweet(tweetId, ++sno));
    }
    
    public List<Integer> getNewsFeed(int userId) {
        LinkedList<Tweet> tweetList = new LinkedList<>();
        if(!userMap.containsKey(userId)){
            userMap.put(userId, new User());
        }
        User user = userMap.get(userId);
        for(Tweet tweet : user.tweet){
            tweetList.addLast(tweet);
        }
        // 线性归并
        for(int followeeId : user.followee){
            if(followeeId == userId){ // 自己关注自己的情况
                continue;
            }
            if(!userMap.containsKey(followeeId)){
                userMap.put(followeeId, new User());
            }
            tweetList = merge(tweetList, userMap.get(followeeId).tweet);
        }
        // 构造答案
        List<Integer> ans = new LinkedList<>();
        for(Tweet tweet : tweetList){
            ans.add(tweet.tweetId);
        }
        return ans;
    }

    private LinkedList<Tweet> merge(LinkedList<Tweet> t1, LinkedList<Tweet> t2){
        LinkedList<Tweet> t = new LinkedList<>();
        int i = 0, j = 0;
        while(i < t1.size() && j < t2.size()){
            if(t1.get(i).tweetSno > t2.get(j).tweetSno){
                t.addLast(t1.get(i));
                i++;
            } else {
                t.addLast(t2.get(j));
                j++;
            }
            if(t.size() >= recentMax){
                break;
            }
        }
        for(; i < t1.size() && t.size() < recentMax; i++){
            t.addLast(t1.get(i));
        }
        for(; j < t2.size() && t.size() < recentMax; j++){
            t.addLast(t2.get(j));
        }
        return t;
    }
    
    public void follow(int followerId, int followeeId) {
        if(!userMap.containsKey(followerId)){
            userMap.put(followerId, new User());
        }
        if(!userMap.containsKey(followeeId)){
            userMap.put(followeeId, new User());
        }
        userMap.get(followerId).followee.add(followeeId);
    }
    
    public void unfollow(int followerId, int followeeId) {
        if(userMap.containsKey(followerId)){
            userMap.get(followerId).followee.remove(followeeId);
        }
    }
}
```

