---
title: leetcode-752-Open the Lock
date: 2022-05-08 15:40:52
summary: 打开转盘锁
categories: leetcode
tags:
- BFS

---
## 打开转盘锁
[leetcode-752-Open the Lock](https://leetcode.cn/problems/open-the-lock/)

### BFS 
> 通俗来说就是枚举

```java
class Solution {
    public static int openLock(String[] deadends, String target) {
        Set<String> deadSet = new HashSet<>(Arrays.asList(deadends));
        if (deadSet.contains("0000") || deadSet.contains(target)){
            return -1;
        }
        if ("0000".equals(target)){
            return 0;
        }
        Queue<String> queue = new LinkedList<>();
        queue.add("0000");
        int count = 0;
        int size = queue.size();
        while (!queue.isEmpty()){

            while (size > 0){
                String s = queue.poll();
                size --;
                String[] res = transfer(s.toCharArray());
                for (String r : res) {
                    if (!deadSet.contains(r)) {
                        if (r.equals(target)){
                            return count + 1;
                        }
                        queue.add(r);
                        deadSet.add(r);
                    }
                }
            }
            size = queue.size();
            count ++;
        }
        return -1;
    }

    private static String[] transfer(char[] str){
        // '9' 57
        // '0' 48
        String[] ans = new String[str.length * 2];
        int cur = 0;
        for (int i = 0; i < str.length; i ++){
            int c = (str[i]);
            // ++
            if (c == 57){
                str[i] = '0';
            }else {
                str[i] = (char) (c + 1);
            }
            ans[cur ++] = new String(str);
            // --
            if (c == 48){
                str[i] = '9';
            }else {
                str[i] = (char) (c - 1);
            }
            ans[cur ++] = new String(str);
            str[i] = (char) c;
        }
        return ans;
    }
}
```
