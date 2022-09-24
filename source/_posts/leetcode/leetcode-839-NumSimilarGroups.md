---
title: leetcode-839-NumSimilarGroups
date: 2022-04-25 16:40:52
summary: 相似字符串组
categories: leetcode
tags:
- 并查集

---
## 相似字符串组
[leetcode-839-NumSimilarGroups](https://leetcode-cn.com/problems/similar-string-groups/)
> 暴力求解是否存在相似 
> 并查集分组

### 并查集

```java
public class NumSimilarGroups {

    public static void main(String[] args) {
        numSimilarGroups(new String[]{"kccomwcgcs","socgcmcwkc","sgckwcmcoc","coswcmcgkc","cowkccmsgc","cosgmccwkc","sgmkwcccoc","coswmccgkc","kowcccmsgc","kgcomwcccs"});
    }

    public static int numSimilarGroups(String[] strs) {
        int[] nums = new int[strs.length];
        for (int i = 0; i < nums.length; i ++){
            nums[i] = i;
        }
        for (int i = 0; i < strs.length; i ++){
            int iP = findParent(nums, i);
            for (int j = i + 1; j < strs.length; j ++){
                if (check(strs[i], strs[j], strs[i].length())){
                    int jP = findParent(nums, j);
                    nums[jP] = iP;
                }
            }
        }
        int count = 0;
        for (int i = 0; i < nums.length; i ++){
            findParent(nums, i);
            if (nums[i] == i){
                count ++;
            }
        }
        return count;
    }

    public static boolean check(String a, String b, int len) {
        int num = 0;
        for (int i = 0; i < len; i++) {
            if (a.charAt(i) != b.charAt(i)) {
                num++;
                if (num > 2) {
                    return false;
                }
            }
        }
        return true;
    }

    private static int findParent(int[] nums, int cur){
        if (nums[cur] == cur){
            return cur;
        }
        nums[cur] = findParent(nums, nums[cur]);
        return nums[cur];
    }

}
```
