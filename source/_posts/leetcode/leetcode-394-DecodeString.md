---
title: leetcode-394-DecodeString
date: 2022-04-28 14:40:52
summary: 字符串解码
categories: leetcode
tags:
- 递归   

---
## 字符串解码
[leetcode-394-DecodeString](https://leetcode-cn.com/problems/decode-string/)
> 遇到 [] 返回字符串
> 递归调用
```java
public class DecodeString {

    int i = 0;
    public String decodeString(String s) {
        char[] str = s.toCharArray();
        int count = 0;
        StringBuilder ans = new StringBuilder();
        while (i < str.length) {
            if (str[i] == '['){
                i ++;
                String s1 =  handSquareBrackets( str);
                for (int j = 0; j < count; j ++){
                    ans.append(s1);
                }
                count = 0;
            }else if (str[i] >= '0' && str[i] <= '9'){
                int c = str[i ++] - '0';
                count = count * 10 + c;
            }else {
                ans.append(str[i ++]);
            }
        }
        return ans.toString();
    }

    private String handSquareBrackets(char[] str){
        int count = 0;
        StringBuilder ans = new StringBuilder();
        while (i < str.length) {
            if (str[i] == '['){
                i ++;
                String s1 =  handSquareBrackets(str);
                for (int j = 0; j < count; j ++){
                    ans.append(s1);
                }
                count = 0;
            }else if (str[i] == ']'){
                i ++;
                return ans.toString();
            } else if (str[i] >= '0' && str[i] <= '9'){
                int c = str[i ++] - '0';
                count = count * 10 + c;
            }else {
                ans.append(str[i ++]);
            }
        }
        return ans.toString();
    }
}
```
