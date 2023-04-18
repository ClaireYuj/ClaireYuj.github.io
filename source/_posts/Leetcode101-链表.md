---
title: Leetcode101--链表
top: false
cover: false
author: DULULU oO
date: 2023-04-03 14:59:16
password:
summary:
tags: leetcode
categories: Leetcode101
---

## [206.反转链表](https://leetcode.cn/problems/reverse-linked-list/submissions/)


![](/img/posts/Leetcode/leetcode206.jpg)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) return head; //初始化判断头和头的下一个是否为空，若空，直接返回
        ListNode cur = new ListNode(head.val); //新建一个cur用来记录现在的

        while(head.next!=null){
            ListNode pre = new ListNode(head.next.val);，//每次新建一个pre，存放head的下一个，然后把pre指向cur，实现翻转
            pre.next = cur;
            head = head.next;
            cur = pre;
        }
        
        return cur;
    }
}

```

## [21.合并有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

![](/img/posts/Leetcode/leetcode21.jpg)

> 动态规划＋递归
 先判断l1.val和l2.val，确定现在是l1还是l2，若是l1, 要确定下一个merge(l1.next,l2)，返回了l1

```java

class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if(list1==null){
            return list2;
        }
        else if(list2 == null){
            return list1;

        }
        else if(list1.val < list2.val){

            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }
        else{
            list2.next = mergeTwoLists(list1,list2.next);
            return list2;
        }
    }
}
```

## [24.两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

![](/img/posts/Leetcode/leetcode24.jpg)

> 逆向思维--递归
 拆分任务--每两个互相交换，也就是每两个都要实现head.next指向swap(next.next),next.next指向head
 确定终止条件 -- head==null||next==null

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode next = head.next;
        head.next = swapPairs(next.next); // head的下一个是next之后的节点继续交换
        next.next = head;

        return next;
    }
}
```

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

![](/img/posts/Leetcode/leetcode24.jpg)

### 双指针
很巧妙的方法，**把指针A和B同时连续遍历A、B两个链表**
pointerA -> AB
pointerB -> BA
假设A到交点需要a步，B到交点需要b步，交点到终点需要b步：
pointerA从起点到第二次走到交点：a+c+b
pointerB从起点到第二次走到交点：b+c+a
第二次就相遇
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode pointerA = headA;
        ListNode pointerB = headB;
        while(pointerA!=pointerB){
            pointerA = pointerA == null? headB:pointerA.next;
            pointerB = pointerB == null? headA:pointerB.next;
        }
        return pointerA;
    }
}
```

## [234.回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。
可以将链表复制到**Arraylist**中国，直接通过下标比较

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ArrayList<Integer> array = new ArrayList<Integer>();
        for(;head!=null;head=head.next){
            array.add(head.val);
        }
        for(int i = 0, j = array.size()-1;i<j;i++,j-- ){
            if(array.get(i)!=array.get(j)) return false;
        }
        return true;
    }
}

```