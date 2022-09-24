---
title: leetcode-433-MinMutation
date: 2022-05-07 14:40:52
summary: 最小基因变化
categories: leetcode
tags: 

---
## 组合
[leetcode-433-MinMutation](https://leetcode-cn.com/problems/minimum-genetic-mutation/)


```java
class Solution {
    public int minMutation(String start, String end, String[] bank) {
        if (start.equals(end)){
            return 0;
        }
        boolean checkInBank = false;
        for (String value : bank) {
            if (end.equals(value)) {
                checkInBank = true;
                break;
            }
        }
        if (!checkInBank){
            return -1;
        }
        Set<String> visited = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        queue.add(end);
        visited.add(end);
        int count = 1;
        int depth = 1;
        while (!queue.isEmpty()){
            while (count > 0){
                String parent = queue.poll();
                if (canTransfer(parent, start)){
                    return depth;
                }
                for (int i = 0; i < bank.length; i ++){
                    String s = bank[i];
                    if (!visited.contains(s) && canTransfer(parent, s)){
                        queue.add(s);
                        visited.add(s);
                    }
                }
                count --;
            }
            depth ++;
            count = queue.size();
        }
        return -1;
    }
    
    private boolean canTransfer(String s1, String s2){
        int count = 0;
        for (int i = 0; i < Math.max(s1.length(), s2.length()); i ++){
            if (s1.charAt(i) != s2.charAt(i)){
                count ++;
                if (count >= 2){
                    return false;
                }
            }
        }
        return true;
    }
}
```
