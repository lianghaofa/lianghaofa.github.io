---
title: Elastic Search 概述
date: 2022-06-01 09:40:52
summary: Elastic Search 概述
categories: Elastic Search
tags:
- Elastic Search 

---
## Elastic Search 概述

### 特点

基于 JSON 的数据库


RESTFul API

### 1

Index

Tables - pattern

Rows

Cols


### 评分算法

7.5之前


TF-IDF

term frequency - inverse document frequency


索引文件

### 正排索引

根据对应的文章ID，查询对应的内容

![正排索引](/medias/NoSQL/1658028543507.jpg)


### 倒排索引

根据对应的内容，查询对应的文章ID

- 分词 

- 去重

- 排序

1658032219761.png
![倒排索引 - 步骤](/medias/NoSQL/1658032219761.jpg)


#### 倒排表


![倒排表](/medias/NoSQL/1655910017756.jpg)

存储 int ? 每个int 4 字节，消耗的存储空间太大了。

倒排表压缩算法

Frame Of Reference

1 2 3 4 5 6 7 ... 100W

73,300,302,332,343,372

73,227,2,30,11,29


73,227    2,30,11,29

len=2,8bit   len=4,bit=5

存储数组长度，存储每个数据占用的位数。bits

![倒排表](/medias/NoSQL/1655951386982.png)



RBM

Container

3种类型的 Container


ArrayContainer - 


1000W,2001W,3003W,5284W,9548W,10212W,21Y 

数值非常大，但个数不多。


BitmapContainer -


1000,62101,131385,132052,191173,196658

8KB - 0 - 63335



RunContainer - 

1，2，3，4 .。。 100W

存储 ： 1- 100W

连续性



1  2 3 5 6 8
存差值。
1 1 1 2 1 2


Roaring BitMaps


词项索引的检索原理


如何计算得分 ？





### FST




词项字典

词项索引


### IK分词器


中文如何进行分词？ 根据分词器 




### Lucene


压缩算法 

- 前缀、后缀压缩
  ![前缀、后缀压缩](/medias/NoSQL/1658070210394.png)

- 差值规则
  ![差值规则](/medias/NoSQL/1658070210394.png)

- 或然跟随规则
  ![或然跟随规则](/medias/NoSQL/1658070933470.png)

- 跳表规则
  ![跳表规则](/medias/NoSQL/1658071153490.png)


### 向量空间模型

