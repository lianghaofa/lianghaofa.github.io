---
title: leetcode-127-WordLadder
date: 2022-04-25 21:40:52
summary: 单词接龙
categories: leetcode
tags:
- BFS   

---
## 单词接龙
[leetcode-127-WordLadder](https://leetcode-cn.com/problems/word-ladder/)

> BFS

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {

        Set<String> words = new HashSet<>(wordList);
        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        int count = 1;
        int d = 1;
        while (!queue.isEmpty()){
            while (count > 0){
                String w = queue.poll();
                if (w.equals(endWord)){
                    return d;
                }
                count --;
                Set<String> temps = new HashSet<>();
                for (String word : words){
                    if (canChange(w, word)){
                        queue.add(word);
                        temps.add(word);
                    }
                }
                for (String word : temps){
                    words.remove(word);
                }
            }
            d ++;
            count = queue.size();
        }
        return 0;
    }

    private boolean canChange(String s1, String s2){
        int count = 0;
        for (int i = 0; i < s1.length(); i ++){
            if (s1.charAt(i) != s2.charAt(i)){
                if (count == 1){
                    return false;
                }else {
                    count ++;
                }
            }
        }
        return count == 1;
    }
}
```
