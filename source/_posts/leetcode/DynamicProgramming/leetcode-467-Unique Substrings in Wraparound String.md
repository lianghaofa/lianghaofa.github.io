---
title: leetcode-467-Unique Substrings in Wraparound String
date: 2022-05-25 09:40:52
summary: 环绕字符串中唯一的子字符串
categories: leetcode
tags:
- DP

---
## 环绕字符串中唯一的子字符串
[leetcode-467-Unique Substrings in Wraparound String](https://leetcode.cn/problems/unique-substrings-in-wraparound-string/)


> 暴力 ： 记录每个位置结尾以及长度相等 的个数  
> 优化 ： bcd ,cd , d 以为 d 为结尾的个数为 3. 

#### 暴力

```java
    public static int findSubstringInWraproundString(String p) {

        char[] chars = p.toCharArray();
        int[] sets = new int[26];
        int ans = 0;
        int k = 1;
        int index =  chars[0] - 'a';
        sets[index] = 1;
        for (int i = 1; i < chars.length; i ++){
            if (chars[i] == chars[i - 1] + 1 || chars[i] == 'a' && chars[i - 1] == 'z'){
                k ++;
            }else {
                k = 1;
            }
            index =  chars[i] - 'a';
            sets[index] = Math.max(sets[index], k);;
        }
        for (int set : sets) {
            ans += set;
        }
        return ans;
    }
```

### 优化

```java
    public static int findSubstringInWraproundString(String p) {

        char[] chars = p.toCharArray();
        boolean[][] sets = new boolean[26][p.length() + 1];
        int ans = 0;
        for (int i = 0; i < chars.length; ){
            if (!sets[chars[i] - 'a'][1]){
                sets[chars[i] - 'a'][1] = true;
                ans ++;
            }
            int j = i + 1;
            while (j < chars.length &&  ((chars[j] == chars[j - 1] + 1) || chars[j] == 'a' && chars[j - 1] == 'z')){
                for (int k = (j - i + 1); k > 0; k --){
                    if (!sets[chars[j] - 'a'][k]){
                        sets[chars[j] - 'a'][k] = true;
                        ans ++;
                    }
                }
                j ++;
            }
            i = j;
        }
        return ans;
    }
```
