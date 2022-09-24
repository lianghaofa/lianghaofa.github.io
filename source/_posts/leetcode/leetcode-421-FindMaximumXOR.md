---
title: leetcode-421-FindMaximumXOR
date: 2022-04-22 12:40:52
summary: 数组中两个数的最大异或值
categories: leetcode
tags:
- Trie

---
## FindMaximumXOR
[leetcode-421-FindMaximumXOR](https://leetcode-cn.com/problems/ms70jA/)

**注意点：任意两个数**
// 数组中最大两个数的异或值
// Trie树 从31位开始建树，代码好写很多！

```java
class Solution {
    class Tried{
        Tried zero;
        Tried one;
        Tried(){
            zero = null;
            one = null;
        }
    }

    public int findMaximumXOR(int[] nums) {
        Tried head = new Tried();
        Tried cur = head, other;
        int x1 = 0x40000000;
        for (int i = 30; i >= 0; i --){
            if ((nums[0] & x1) != 0){
                cur.one = new Tried();
                cur = cur.one;
            }else {
                cur.zero = new Tried();
                cur = cur.zero;
            }
            x1 >>= 1;
        }
        int max = 0;
        for (int i = 1; i < nums.length; i ++){
            cur = head;
            other = head;
            int m = 0;
            int x = 0x40000000;
            for (int j = 30; j >= 0; j --){
                if ((nums[i] & x) != 0){
                    if (other.zero != null){
                        other = other.zero;
                        m |= x;
                    }else {
                        other = other.one;
                    }
                    if (cur.one == null){
                        cur.one = new Tried();
                    }
                    cur = cur.one;
                }else {
                    if (other.one != null){
                        other = other.one;
                        m |= x;
                    }else {
                        other = other.zero;
                    }
                    if (cur.zero == null){
                        cur.zero = new Tried();
                    }
                    cur = cur.zero;
                }
                x >>= 1;
            }
            max = Math.max(max,m);
        }
        return max;
    }

}
```