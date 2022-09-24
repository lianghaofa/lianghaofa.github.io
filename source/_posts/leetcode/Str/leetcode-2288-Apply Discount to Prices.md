---
title: leetcode-2288-Apply Discount to Prices
date: 2022-06-16 15:40:52
summary: 价格减免
categories: leetcode
tags:


---
## 价格减免
[leetcode-2288-Apply Discount to Prices](https://leetcode.cn/problems/apply-discount-to-prices/)


```java
class Solution {
    private static final DecimalFormat df = new DecimalFormat("0.00");

    public String discountPrices(String sentence, int discount) {

        // 减免 discount%
        char[] chars = sentence.toCharArray();
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < chars.length; ){
            if (i > 0 &&  chars[i - 1] == ' ' &&   chars[i] == '$' || i == 0 && chars[i] == '$'){
                int j = i + 1;
                double c = 0;
                while (j <= chars.length){
                    if (j == chars.length){
                        c -= c * (discount / 100d);
                        //new BigDecimal(c);
                        if (j == i + 1){
                            ans.append("$");
                        }else {
                            ans.append("$").append(df.format(c));
                        }
                        i = j + 1;
                        break;
                    } else if (chars[j] == ' '){
                        c -= c * (discount / 100d);
                        if (j == i + 1){
                            ans.append("$").append(' ');
                        }else {
                            ans.append("$").append(df.format(c)).append(' ');
                        }
                        i = j + 1;
                        break;
                    } else if (chars[j] >= '0' && chars[j] <= '9'){
                        c = c * 10 + (chars[j] - '0');
                    }else {
                        // i - j
                        ans.append(new String(chars, i , j - i + 1));
                        i = j + 1;
                        break;
                    }
                    j ++;
                }
            }else {
                ans.append(chars[i]);
                i ++;
            }

        }
        return ans.toString();
    }
}
```
