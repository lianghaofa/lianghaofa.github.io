---
title: leetcode-1797-Design Authentication Manager
date: 2022-06-23 14:40:52
summary: 设计一个验证系统
categories: leetcode
tags:
- Hash   

---
## 设计一个验证系统
[leetcode-1797-Design Authentication Manager](https://leetcode.cn/problems/design-authentication-manager/)



```java
class AuthenticationManager {

    int timeToLive;
    Map<String,Integer> map;
    public AuthenticationManager(int timeToLive) {
        this.timeToLive = timeToLive;
        map = new HashMap<>();

    }

    public void generate(String tokenId, int currentTime) {
        map.put(tokenId,currentTime);
    }

    public void renew(String tokenId, int currentTime) {
        Integer val = map.get(tokenId);
        if (val != null){

            int a = val + timeToLive;
            if (a > currentTime){
                generate(tokenId, currentTime);
            }
        }
    }

    public int countUnexpiredTokens(int currentTime) {

        int ans = 0;
        List<String> deletes = new ArrayList<>();
        for (Map.Entry<String, Integer> entry : map.entrySet()){
            int a = entry.getValue() + timeToLive;
            if (a > currentTime){
                ans ++;
            }else {
                deletes.add(entry.getKey());
            }
        }
        for (String s : deletes){
            map.remove(s);
        }
        return ans;
    }
}

```
