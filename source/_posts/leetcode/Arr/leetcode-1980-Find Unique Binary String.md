---
title: leetcode-1980-Find Unique Binary String
date: 2022-06-05 14:20:52
summary: 找出不同的二进制字符串
categories: leetcode
tags: 

---
## 找出不同的二进制字符串
[leetcode-1980-Find Unique Binary String](https://leetcode.cn/problems/find-unique-binary-string/)




```java
public class FindDifferentBinaryString {

    public String findDifferentBinaryString(String[] nums) {

        if (nums.length == 0){
            return "";
        }
        char[] res = new char[nums[0].length()];
        for (int i = 0; i < nums.length; i ++){

            char c = nums[i].charAt(i);
            res[i] = c == '1' ? '0' : '1';
        }
        return new String(res);
    }
}
```
