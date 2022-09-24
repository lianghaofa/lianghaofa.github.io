---
title: leetcode-820-FindMaximumXOR
date: 2022-04-22 11:40:52
summary: 单词的压缩编码
categories: leetcode
tags:
- Trie
- DFS
- BFS
---
## MinimumLengthEncoding
[leetcode-820-FindMaximumXOR](https://leetcode-cn.com/problems/short-encoding-of-words/)

**逆序建立字典序**
// 1.逆序建立字典序。因为只需要统计长度，长的可以把短的直接覆盖
// 2.DFS或BFS统计长度

```java
class Solution {
    class Node{
        Node[] children;
        Node(){
            children = new Node[26];
        }
    }

    // 1.逆序建立字典序。因为只需要统计长度，长的可以把短的直接覆盖
    // 2.DFS或BFS统计长度
    int ans = 0;
    public int minimumLengthEncoding(String[] words) {
        Node head = new Node();
        for (String word : words) {
            Node cur = head;
            String s = new StringBuilder(word).reverse().toString();
            for (int j = 0; j < s.length(); j++) {
                if (cur.children[s.charAt(j) - 'a'] == null) {
                    cur.children[s.charAt(j) - 'a'] = new Node();
                }
                cur = cur.children[s.charAt(j) - 'a'];
            }
        }
        dfs(head, 0);
        return ans;
    }

    private void dfs(Node cur, int depth){
        Node[] children = cur.children;
        boolean empty = true;
        for (Node child : children) {
            if (child != null) {
                empty = false;
                dfs(child, depth + 1);
            }
        }
        if (empty){
            ans += depth + 1;
        }
    }
}
```