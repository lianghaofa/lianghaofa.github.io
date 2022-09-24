---
title: leetcode-140-WordBreak
date: 2022-05-07 16:40:52
summary: 单词拆分 II
categories: leetcode
tags:
- DP

---
## 单词拆分 II
[leetcode-140-WordBreak](https://leetcode-cn.com/problems/word-break-ii/)


### DP

```java
class Solution {
    List<String> ans;
    public List<String> wordBreak(String s, List<String> wordDict) {
        ans = new ArrayList<>();
        Set<String> dict = new HashSet<>(wordDict);
        List<String> res = new ArrayList<>();
        dfs(s, 0, dict,  res);
        return ans;
    }

    private void dfs(String s, int index, Set<String> dict, List<String> res){
        if (index == s.length()){
            StringBuilder r = new StringBuilder();
            for (String re : res) {
                r.append(re).append(" ");
            }
            ans.add(r.toString().trim());
            return;
        }
        StringBuilder builder = new StringBuilder();
        for (int i = index; i < s.length(); i ++){
            builder.append(s.charAt(i));
            if (dict.contains(builder.toString())){
                res.add(builder.toString());
                dfs(s, i + 1, dict, res);
                res.remove(res.size() - 1);
            }
        }
    }
}
```
