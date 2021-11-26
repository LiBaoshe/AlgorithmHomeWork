#### [811. 子域名访问计数](https://leetcode-cn.com/problems/subdomain-visit-count/)

算法：遍历字符串数组，先用空格分割字符串得到计数和域名，再根据点分割域名，使用HashMap累加保存域名出现的次数，遍历完数组后将map中的统计结果映射到List中返回。

时间复杂度：O(n)

空间复杂度：O(n)

实现代码：

```java
class Solution {
    public List<String> subdomainVisits(String[] cpdomains) {
        List<String> ans = new LinkedList<>();
        Map<String, Integer> map = new HashMap<>();
        for(String cpdomain : cpdomains){
            String[] strs = cpdomain.split(" ");
            int count = Integer.parseInt(strs[0]);
            String domain = strs[1];
            int start = 0;
            do {
                domain = domain.substring(start);
                map.put(domain, map.getOrDefault(domain, 0) + count);
                start = domain.indexOf('.') + 1;
            } while(start > 0);
        }
        for(Map.Entry<String, Integer> entry : map.entrySet()){
            ans.add(entry.getValue() + " " + entry.getKey());
        }
        return ans;
    }
}
```

