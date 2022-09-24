---
title: leetcode-2284-Sender With Largest Word Count
date: 2022-06-17 21:40:52
summary: 最多单词数的发件人
categories: leetcode
tags:


---
## 最多单词数的发件人
[leetcode-2284-Sender With Largest Word Count](https://leetcode.cn/problems/sender-with-largest-word-count/)


```java
class Solution {
    class Node{
        String name;
        int cnt;
        Node(String name, int cnt){
            this.name = name;
            this.cnt = cnt;
        }

    }

    public String largestWordCount(String[] messages, String[] senders) {

        HashMap<String, Node> hashMap = new HashMap<>();
        for (int i = 0; i < messages.length; i ++){
            String[] words = messages[i].trim().split(" ");
            String name = senders[i];
            Node node = hashMap.get(name);
            if (node == null){
                hashMap.put(name, new Node(name, words.length));
            }else {
                node.cnt += words.length;
            }
        }

        int cnt = Integer.MIN_VALUE;
        String name = "";

        for (Map.Entry<String, Node> entry : hashMap.entrySet()){
            Node node = entry.getValue();
            if (cnt == Integer.MIN_VALUE){
                cnt = node.cnt;
                name = node.name;
            }else if (cnt < node.cnt){
                cnt = node.cnt;
                name = node.name;
            }else if (cnt == node.cnt && !bigger(name.toCharArray(), node.name.toCharArray())){
                cnt = node.cnt;
                name = node.name;
            }
        }


        return name;
    }


    private boolean bigger(char[] s0, char[] s1){
        
        for (int i = 0; i < s0.length && i < s1.length; i ++){
            if (s0[i] > s1[i]){
                return true;
            }else if (s0[i] < s1[i]){
                return false;
            }
        }
        return s0.length > s1.length;
    }
}
```
