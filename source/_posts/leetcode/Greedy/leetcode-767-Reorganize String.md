---
title: leetcode-767-Reorganize String
date: 2022-05-31 19:10:52
summary: 重构字符串
categories: leetcode
tags:
- 贪心   

---
## 重构字符串
[leetcode-767-Reorganize String](https://leetcode.cn/problems/reorganize-string/)


### 贪心

```java
class Solution {

    static class Node{
        char c;
        int cnt;
        Node(char c, int cnt){
            this.c = c;
            this.cnt = cnt;
        }
    }

    public static String reorganizeString(String s) {
        if (s.length() <= 1){
            return s;
        }
        int[] arr = new int[26];
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < s.length(); i ++){
            int index = s.charAt(i) - 'a';
            arr[index] ++;
            max = Math.max(max, arr[index]);
        }
        List<List<Character>> lists = new ArrayList<>();
        for (int i = 0; i < max; i ++){
            lists.add(new ArrayList<>());
        }
        char[] ans = new char[s.length()];
        int index = 0;
        PriorityQueue<Node> priorityQueue = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                return o2.cnt - o1.cnt;
            }
        });
        for (int i = 0; i < arr.length; i ++){
            if (arr[i] > 0){
                priorityQueue.add(new Node((char)('a' + i), arr[i]));
            }
        }
        while (!priorityQueue.isEmpty()){
            Node node = priorityQueue.poll();
            for (int i = 0; i < node.cnt; i ++){
                int bucket = index % max;
                lists.get(bucket).add(node.c);
                index ++;
            }
        }

        if (lists.size() > 1 && lists.get(lists.size() - 2).size() == 1){
            return "";
        }
        index = 0;
        for (int i = 0; i < max; i ++){
            List<Character> list = lists.get(i);
            for (Character character : list) {
                ans[index++] = character;
            }
        }

        return new String(ans);
    }

}
```
