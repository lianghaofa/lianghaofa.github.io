---
title: leetcode-421-MapSum
date: 2022-04-22 16:40:52
summary: 键值映射
categories: leetcode
tags:
- Trie

---
## MapSum
[leetcode-677-FindMaximumXOR](https://leetcode-cn.com/problems/map-sum-pairs/)

**void insert(String key, int val) 如果键 key 已经存在，那么原来的键值对将被替代成新的键值对**


```java
public class MapSum {

    class Trie {
        int count;
        Trie[] children;
        Trie(int count){
            this.count = count;
            children = new Trie[26];
        }
    }
    Trie head;
    Map<String,Integer> map;
    /** Initialize your data structure here. */
    public MapSum() {
        head = new Trie(0);
        map = new HashMap<>();
    }



    public void insert(String key, int val) {
        Trie cur = head;
        boolean existed =  false;
        Integer v = map.get(key);
        map.put(key,val);
        for (int i = 0; i < key.length(); i ++){
            if (cur.children[key.charAt(i) - 'a'] == null){
                cur.children[key.charAt(i) - 'a'] = new Trie(val);
            }else {
                if (v == null){
                    cur.children[key.charAt(i) - 'a'].count += val;
                }else {
                    cur.children[key.charAt(i) - 'a'].count +=  (v - val);
                }
            }
            cur = cur.children[key.charAt(i) - 'a'];
        }
    }

    public int sum(String prefix) {
        Trie cur = head;
        int ans = 0;
        for (int i = 0; i < prefix.length(); i ++){
            if (cur.children[prefix.charAt(i) - 'a'] == null){
                return ans;
            }
            cur = cur.children[prefix.charAt(i) - 'a'];
        }
        return cur.count;
    }
}
```