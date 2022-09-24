---
title: leetcode-763-PartitionLabels
date: 2022-04-27 21:40:52
summary: 划分字母区间
categories: leetcode
tags:
- 贪心   

---
## 组合
[leetcode-763-PartitionLabels](https://leetcode-cn.com/problems/partition-labels/)

> BFS

```java
public class PartitionLabels {

    public static void main(String[] args) {
        partitionLabels("ababcbacadefegdehijhklij");
    }
    public static List<Integer> partitionLabels(String s) {
        char[] sArr = s.toCharArray();
        List<Integer> ans = new ArrayList<>();
        int[] lastIndexArr = new int[26];
        for (int i = 0; i < s.length(); i ++){
            lastIndexArr[sArr[i] - 'a'] = i;
        }
        int pre = 0;
        int lastIndex = lastIndexArr[sArr[0] - 'a'];
        for (int i = 0; i < s.length(); i ++){
            lastIndex = Math.max(lastIndex, lastIndexArr[sArr[i] - 'a']);
            if (lastIndex == i){
                ans.add(i - pre);
                pre = i + 1;
            }
        }
        return ans;
    }
}
```
