---
title: leetcode-1898-Maximum Number of Removable Characters
date: 2022-05-14 19:50:52
summary: 搜索旋转排序数组
categories: leetcode
- 二分

---
## 可移除字符的最大数目


[leetcode-1898-Maximum Number of Removable Characters](https://leetcode.cn/problems/maximum-number-of-removable-characters/)


```java
class Solution {
    public int maximumRemovals(String s, String p, int[] removable) {
        
        char[] pArr = p.toCharArray();
        int l = 1;
        int r = removable.length;
        int ans = 0;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (check(s.toCharArray(), pArr, mid, removable)){
                ans = mid;
                l = mid + 1;
            }else {
                r = mid - 1;
            }
        }
        return ans;
    }

    private boolean check(char[] s, char[] p, int mid, int[] removable){
        //这里做的就是将前mid个removable中所指的字符"删除"
        for (int i = 0;i < mid;i ++) {
            s[removable[i]] = '1';
        }
        int j = 0;
        //判断p是不是s的子序列
        for (int i = 0;i < s.length;i ++) {
            if (j < p.length) {
                if (p[j] == s[i]) {
                    j ++;
                }
                //已经遍历完p，即可说明p是s的子序列
            }else {
                return true;
            }
        }
        //已经遍历完p，即可说明p是s的子序列
        if (j == p.length) {
            return true;
        }
        //已经遍历完s但是没有完全找到p对应的字符，即p不是s的子序列。
        return false;
    }
}
```
