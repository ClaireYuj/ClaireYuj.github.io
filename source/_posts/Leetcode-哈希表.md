---
title: Leetcode-哈希表
top: false
cover: false
author: DULULU oO
date: 2021-07-30 11:11:41
password:
summary:
tags: 
    - leetcode
categories: 数据结构
---


## 构造哈希表
参考[Leetcode705-设计哈希结合](https://leetcode.cn/problems/design-hashset/)

要解决的问题
- 映射： 将集合中任意可能的元素映射到一个固定范围的整数值，并将该元素存储到整数值对应的地址上。
- 冲突处理：由于不同元素可能映射到相同的整数值，因此需要在整数值出现「冲突」时，需要进行冲突处理
- 扩容：当哈希表元素过多时，冲突的概率将越来越大，而在哈希表中查询一个元素的效率也会越来越低。因此，需要开辟一块更大的空间，来缓解哈希表中发生的冲突。

考虑到哈希表后期需要扩容，一般不会考虑直接用数组，而是采用链表

## 