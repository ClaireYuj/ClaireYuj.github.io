---
title: Leetcode-链表相关
top: false
cover: false
author: DULULU oO
date: 2021-08-28 20:13:47
password:
summary:
tags: leetcode
categories: 数据结构
---

## 剑指 Offer 24. 反转链表

[剑指Offer24.反转链表](https://leetcode.cn/leetbook/read/illustration-of-algorithm/9pdjbm/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
如：输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

### 解法一，头插：用一个节点暂存

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null) return head;
        ListNode reve = new ListNode(head.val);
        ListNode tmp = null;
        while(head.next!=null){
            tmp = new ListNode(head.next.val);
            tmp.next = reve;
            reve = tmp;
            head = head.next;
        }
        return reve;

    }
}
```
### 解法二：递归

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        return recur(null,head);
    }

    public ListNode recur(ListNode cur,ListNode head){
        if(head==null) return cur; //递归终止条件，所以最后返回的是cur
        ListNode res = recur(head,head.next);
        head.next = cur; //
        return res;
    }
}

```

## 赋值带随机指针的链表

![Leetcode138](/img/posts/Leetcode/leetcode138.jpg)

[解法](https://leetcode.cn/leetbook/read/illustration-of-algorithm/9plk45/)

### 解法一：拼接+拆分

将原链表假设为1->2->3复制
1->1->2->2->3->3->null
这样cur->random->next = cur->next->random
比如如果1->random = 3,那么1->random = 3, 3->next = 3 ===>1->random->next = 3, 且1->next = 1, 1->random = 3 ===> 1->next->random = 3, 所以cur->next->random = cur->random->next

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if(head == null) return head;
        Node cur = head;

        Node p = cur;
        //1. 复制节点并拼接
        while(cur!=null){ 
            Node copy = new Node(cur.val);  //复制该节点
            copy.next = cur.next; //连接copy节点的next
            cur.next = copy; //关联该节点与复制节点
            cur = copy.next; //移动指针
        }

        // 2.构建random,使得cur->random->next = cur->next->random
        cur = head; // java中对象传递是引用传递
        while(cur!=null){
            if(cur.random!=null){
                cur.next.random = cur.random.next;  
            }
            cur = cur.next.next;
        }

        // 3. 将random拆出来
        cur = head.next;
        Node pre = head,res=head.next;
        while(cur.next!=null){
            /**注意pre和cur节点的顺序， pre要在cur之前，
             */
            pre.next = pre.next.next; 
            cur.next = cur.next.next;
            pre = pre.next;
            cur = cur.next;
          
           
        }
        pre.next = null;
        return res;

    }
}
```

### 解法二：哈希表