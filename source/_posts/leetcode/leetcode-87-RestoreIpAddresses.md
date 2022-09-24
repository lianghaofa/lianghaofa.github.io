---
title: leetcode-87-RestoreIpAddresses
date: 2022-04-230 20:40:52
summary: 复原 IP
categories: leetcode
tags: 

---
## 组合
[leetcode-87-RestoreIpAddresses](https://leetcode-cn.com/problems/0on3uN/)


```java
public class RestoreIpAddresses {
    
    List<String> ans = new ArrayList<>();
    public  List<String> restoreIpAddresses(String s) {

        restoreIpAddresses( s, 0,  0, new StringBuilder());
        return ans;
    }

    public  void restoreIpAddresses(String s, int index, int len, StringBuilder builder) {
        if (len == 4){
            if (index == s.length()){
                ans.add(builder.toString());
            }else {
                return;
            }
        }
        if (index >= s.length()){
            return;
        }
        // 截取1位
        int bLen = builder.length();
        builder.append(s.substring(index, index + 1));
        if (len < 3){
            builder.append(".");
        }
        restoreIpAddresses( s, index + 1, len + 1, builder);
        builder.delete(bLen, builder.length());
        // 截取2位
        if (index < s.length() - 1 && s.charAt(index) != '0'){
            builder.append(s.substring(index, index + 2));
            if (len < 3){
                builder.append(".");
            }
            restoreIpAddresses( s, index + 2, len + 1, builder);
            builder.delete(bLen, builder.length());
        }
        // 截取3位
        if ( index < s.length() - 2 &&  index + 2 < s.length() && s.charAt(index) != '0'){
            int val = Integer.parseInt(s.substring(index, index + 3));
            if (val > 255 || val < 100 ){
                return;
            }
            builder.append(val);
            if (len < 3){
                builder.append(".");
            }
            restoreIpAddresses( s, index + 3, len + 1, builder);
            builder.delete(bLen, builder.length());
        }
    }
}
```
