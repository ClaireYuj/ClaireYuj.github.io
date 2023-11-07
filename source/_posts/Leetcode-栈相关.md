---
title: Leetcode-栈相关
top: false
cover: false
author: DULULU oO
date: 2021-09-09 21:30:37
password:
summary:
tags: leetcode
categories: 数据结构
---

栈在JAVA中，可以用LinkedList来表示

LinkedList<Integer> A = new LinkedList<Integer>(); 
- 栈中加入元素
    A.add(a);
- 栈中移除元素
    A.removeLast();

## 剑指 Offer 09. 用两个栈实现队列

https://leetcode.cn/leetbook/read/illustration-of-algorithm/5d3i87/

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

![剑指offer09](/img/posts/Leetcode/offer09.jpg)

```java
/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
class CQueue {

    LinkedList<Integer> A;
    LinkedList<Integer> B;
    public CQueue() {
        A = new LinkedList<Integer>();
        B = new LinkedList<Integer>();
    }
    
    public void appendTail(int value) {
        A.add(value);
    }
    
    public int deleteHead() {
        if(!B.isEmpty()) return B.removeLast();
        if(A.isEmpty()) return -1;
        while(!A.isEmpty())
            B.add(A.removeLast());
        return B.removeLast();
    }
}

```