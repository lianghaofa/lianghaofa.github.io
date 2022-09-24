---
title: leetcode-68-Text Justification
date: 2022-05-29 09:40:52
summary: 文本左右对齐
categories: leetcode
tags:
- DP

---
## 文本左右对齐
[leetcode-68-Text Justification](https://leetcode.cn/problems/text-justification/)


#### 文本左右对齐

```java
class Solution {
    public static List<String> fullJustify(String[] words, int maxWidth) {
        List<String> ans = new ArrayList<>();

        int start = 0;
        int end = 0;
        int len = end - start;
        while (end < words.length){
            int l = words[end].length() + len + (Math.max(end - start, 0));
            if (l == maxWidth){
                // central
                if (end == words.length - 1){
                    ans.add(centralize(words, start, end, words[end].length() + len, maxWidth, true));
                }else {
                    ans.add(centralize(words, start, end, words[end].length() + len, maxWidth, false));
                }
                start = end + 1;
                len = 0;
            }else if (l > maxWidth){
                ans.add(centralize(words, start, end - 1, len, maxWidth, false));
                end --;
                start = end + 1;
                len = 0;
            }else if (end == words.length - 1){
                ans.add(centralize(words, start, end, words[end].length() + len,maxWidth, true));

            }else {
                len += words[end].length();
            }
            end ++;
        }
        return ans;
    }


private static String centralize(String[] words, int l, int r, int len, int maxWidth, boolean endLine){

        char[] chars = new char[maxWidth];
        Arrays.fill(chars, ' ');
        if (endLine){
            int start = 0;
            for (int i =  l; i <= r; i ++){
                for (int j = 0; j < words[i].length(); j ++){
                    chars[start ++] = words[i].charAt(j);
                }
                start ++;
            }
        }else {
            int start = 0;
            int end = maxWidth - 1;
            int space = (r - l) == 0 ? 0 : (maxWidth - len) / (r - l);
            int last = (r - l) == 0 ? 0 : (maxWidth - len) % (r - l);
            while (l <= r){
                while (l <= r){
                    for (int j = 0; j < words[l].length(); j ++){
                        chars[start ++] = words[l].charAt(j);
                    }
                    start += space;
                    if (last > 0){
                        start ++;
                        last --;
                    }
                    l ++;
                }
                while (l < r){
                    for (int j = words[r].length() - 1; j >= 0; j --){
                        chars[end --] = words[r].charAt(j);
                    }
                    end -= space;
                    if (last > 0){
                        end --;
                        last --;
                    }
                    r --;
                }
            }
        }
        return new String(chars);
    }
}
```
