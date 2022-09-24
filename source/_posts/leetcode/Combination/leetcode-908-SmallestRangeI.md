---
title: leetcode-40-combinationSum2
date: 2022-04-30 19:40:52
summary: 组合总和 II
categories: leetcode
- 组合
- DFS

---
## 最小差值 I

> 如果当前位置不要，那么等于当前位置的数，都不应该要。

[leetcode-40-combinationSum2](https://leetcode-cn.com/problems/combination-sum-ii/)


```java
public class CombinationSum2 {

    List<List<Integer>> ans = new ArrayList<>();
    int[] nextArr;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {

        Arrays.sort(candidates);
        nextArr = new int[candidates.length];
        int s = 0;
        while (s < candidates.length){
            int end = getNextDifferentIndex( candidates, s);
            for (int i = s; i < end; i ++){
                nextArr[i] = end;
            }
            s = end;
        }

        combinationSum(candidates, target, 0, 0, new ArrayList<>());
        return ans;
    }


    private void combinationSum(int[] candidates, int target, int start, int sum, List<Integer> res) {
        if (sum > target){
            return;
        }
        if (sum == target){
            ans.add(new ArrayList<>(res));
            return;
        }
        if (start >= candidates.length){
            return;
        }
        // 如果当前位置不要，那么等于当前位置的数，都不应该要。
        combinationSum(candidates, target, nextArr[start], sum ,  res);
        res.add(candidates[start]);
        combinationSum(candidates, target, start + 1, sum + candidates[start],  res);
        res.remove(res.size() - 1);

    }

    private int getNextDifferentIndex(int[] candidates, int cur){
        int target = candidates[cur];
        int ans = cur + 1;
        int l = cur + 1;
        int r = candidates.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (candidates[mid] == target){
                ans = mid + 1;
                l = mid + 1;
            }else if (candidates[mid] > target){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }
}
```
