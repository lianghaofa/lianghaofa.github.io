---
title: leetcode-1286-Iterator for Combination
date: 2022-06-19 15:20:52
summary: 字母组合迭代器
categories: leetcode
tags: 

---
## 字母组合迭代器
[leetcode-1286-Iterator for Combination](https://leetcode.cn/problems/iterator-for-combination/)





```java
public class CombinationIterator {

    char[] characters;
    int[] pos;
    int end;

    public static void main(String[] args) {
//        CombinationIterator iterator = new CombinationIterator("chp", 1);
//        iterator.hasNext();
//        iterator.next();
//        iterator.hasNext();
//        iterator.hasNext();
//        iterator.next();
//        iterator.next();
//        iterator.hasNext();
//        iterator.hasNext();
//        iterator.hasNext();
//        iterator.hasNext();

        CombinationIterator iterator = new CombinationIterator("abc", 2);
        System.out.println(iterator.next());

        System.out.println(iterator.hasNext());

        System.out.println(iterator.next());

        System.out.println(iterator.hasNext());

        System.out.println(iterator.next());

        System.out.println(iterator.hasNext());

    }


    public CombinationIterator(String characters, int combinationLength) {
        this.characters = characters.toCharArray();
        this.pos = new int[combinationLength];
        for (int i = 0; i < combinationLength; i ++){
            pos[i] = i;
        }
        end = characters.length() - combinationLength;
    }

    public String next() {
        if (hasNext()){
            StringBuilder ans = new StringBuilder();
            for (int po : pos) {
                ans.append(characters[po]);
            }
            removeToNext(pos.length - 1);
            return ans.toString();
        }else {
            return "";
        }
    }

    public boolean hasNext() {
        return pos[0] <= end;
    }

    private void removeToNext(int index){

        if (pos[0] > end){
            return;
        }
        if (index == -1){
            pos[0] ++;
            return;
        }
        int e = characters.length - (pos.length - index);
        if (pos[index] < e){
            int s = pos[index];
            for (int i = index; i < pos.length; i ++){
                pos[i] = ++ s;
            }
            return;
        }
        removeToNext(index - 1);
    }
}

```
