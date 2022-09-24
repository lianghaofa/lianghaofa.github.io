---
title: leetcode-468-Validate IP Address
date: 2022-05-25 20:40:52
summary: 验证IP地址
categories: leetcode
tags:


---
## 验证IP地址
[leetcode-468-Validate IP Address](https://leetcode.cn/problems/validate-ip-address/)


```java
class Solution {
    public static String validIPAddress(String queryIP) {

        if (queryIP.indexOf('.') != -1){
            return isIPv4(queryIP);
        }else if (queryIP.indexOf(':') != -1){
            return isIPv6(queryIP);
        }else {
            return "Neither";
        }
    }

    private static String isIPv6(String ip) {
        if (ip.length() <= 1 || ip.charAt(0) == ':' || ip.charAt(ip.length() - 1) == ':'){
            return "Neither";
        }
        String[] strs = ip.split(":");
        if (strs.length == 8){
            for (String str : strs){
                if (str.length() < 1 || str.length() > 4){
                    return "Neither";
                }else {
                    for (int i = 0; i < str.length(); i ++){
                        char c = str.charAt(i);
                        if (!(c >= '0' && c <= '9' || c >= 'a' && c <= 'f' || c >= 'A' && c <= 'F')){
                            return "Neither";
                        }
                    }
                }
            }
            return "IPv6";
        }
        return "Neither";
    }

    private static String isIPv4(String ip) {
        if (ip.length() <= 1 || ip.charAt(0) == '.' || ip.charAt(ip.length() - 1) == '.'){
            return "Neither";
        }
        String[] strs = ip.split("\\.");
        if (strs.length == 4){
            for (String str : strs){
                if (str.length() <= 0 || str.length() > 3){
                    return "Neither";
                }
                for (int i = 0; i < str.length(); i ++){
                    char ch = str.charAt(i);
                    if (ch > '9' || ch < '0'){
                        return "Neither";
                    }
                }
                if (str.charAt(0) == '0' && str.length() > 1 || Integer.parseInt(str) > 255){
                    return "Neither";
                }
            }
            return "IPv4";
        }
        return "Neither";
    }
}
```
