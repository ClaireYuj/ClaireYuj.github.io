---
title: Leetcode--双指针
top: false
cover: false
author: DULULU oO
date: 2023-02-11 18:27:33
password:
summary:
tags: leetcode
categories: leetcode101
---

## 一些Java中指针的基本概念

java“指针”就是对象的引用，是存放在堆中的，因为Java中对象是存放在堆中。我们知道java中的内存分为堆内存（heap）和栈内存（stack）。堆就是用来存放对象的，而栈则是存放一些数据基本类型的值，如int,float,double,char.......

### Java堆

JVM只有一个堆区，在虚拟机启动时创建，被所有线程共享，堆区不放基本类型（成员变量除外）和对象的引用，只存储对象本身（包括class对象和异常对象）和数组，堆是GC所管理的主要区域（对不需要的对象进行标记，而后进行清除）
堆是用来存放程序动态生成的数据。（**new 出来的对象的实例存储在堆中**，但是**仅仅**存储的是成员**变量**，也就是平时所说的实例变量，**成员变量的值**则存储在常量池中。成员方法是此类所实现实例共享的，并不是每一次new 都会创建成员方法。成员方法被存储在方法区，并不是存储在第一个创建的对象中，因为那样的话，第一个对象被回收，后面创建的对象也就没有方法引用了。静态变量也存储在方法区中。局部变量在栈内存中，JVM为每一个类分配一个栈帧，然后引用类型的局部变量指向堆内存中的地址），堆是内存中共享的区域，要考虑线程安全的问题。

### Java栈

用来存放基本数据类型和引用数据类型的实例的（也就是实例对象的在堆中的首地址，Person p = new Person; p存贮在堆栈中,值为@23651dff。还有就是堆栈是线程独享的。每一个线程都有自己的线程栈。

### ==与equal

- ‘==’比较的是地址

- equals比较的是内容


## 167. 两数之和 II - 输入有序数组

[题目](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted):给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序排列  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。
以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。
你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。你所设计的解决方案必须只使用常量级的额外空间。

![Leetcode167图示](/img/posts/Leetcode/leetcode167.jpg)

### 暴力解决法

虽然不太聪明，但很暴力

#### 思路

直接用index1和index2， 如果
1. number[index1]+number[index2]<target，index2右移
2.  number[index1]+number[index2]>target, index1右移， 且index2归位，index2=index1+1
3. number[index1]+number[index2]=target, 返回该值

#### 代码

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
      int index_1 = 0,index_2 = 1;
        for(; index_1 < numbers.length;){
            int sum = numbers[index_1]+numbers[index_2];
            if(sum < target&&index_2 < numbers.length-1)  {
                index_2++; 
            }
            else if(sum == target) {
                index_1++;
                index_2++;
                break;
            }
            else{
                index_1++;
                index_2 = index_1+1;
            }
        }
        int[] results = {index_1,index_2};
        return results;

    }
}
```


### 双指针法（推荐）

注意该数组是一个有序的递增数组，所以可以用该方法

```java
public int[] twoSum(int[] numbers, int target) {
    int i = 0;
    int j = numbers.length - 1;
    while (i < j) {
        int sum = numbers[i] + numbers[j];
        if (sum < target) {
            i++;
        } else if (sum > target) {
            j--;
        } else {
            return new int[]{i+1, j+1};
        }
    }
    return new int[]{-1, -1};
}
```



## 剑指offer18： 删除链表中的节点

[题目](https://leetcode.cn/leetbook/read/illustration-of-algorithm/509cy5/)
给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

注意：此题对比原题有改动

示例 1:

输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
示例 2:

输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.


### 单指针法

构造一个虚拟链表 cur 以 0 开始，并让该链表指向头指针 cur.next = head; 假设 head 为 **1->2->3->4**
此时 cur = 0->1->2->3->4

若要删除数字3，需要利用**cur.next.val**来判断是否与val相等, 当cur.next.val = 3时，此时cur = 2->3->4
删除3要求，cur->next = cur->next->next
cur = 2->4, 同时head中的3也被删去了


```java
    public ListNode deleteNode(ListNode head, int val){
        ListNode cur = new ListNode(0);
        cur.next = head;
        if(head.val == val){
            return head.next;
        }
        for(;cur.next != null;){
            if(cur.next.val == val){
                cur.next = cur.next.next;
                break;
            }
            cur = cur.next;
        }
        return head;
    }

```

### 双指针法

```java
 public ListNode deleteNode(ListNode head, int val) {
        ListNode pre = head;
        ListNode cur = head.next;
        if(head.val == val){
            return head.next;
        }
        for(;cur != null;){
            if(cur.val == val){
                cur = cur.next;
                break;
            }
            pre = pre.next;
            cur = cur.next;
        }
        pre.next = cur;
        return head;
}
```

## [88.合并两个数组](https://leetcode.cn/problems/merge-sorted-array/)

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 

> 示例：
 输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
 输出：[1,2,2,3,5,6]
 解释：需要合并 [1,2,3] 和 [2,5,6] 。
 合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。


### 暴力解法

虽然有点笨，但有用
```java

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
            int arr[] = new int[m+n];
            int index_1 = 0, index_2 = 0;
            if(n == 0){
                return;
            }
            if(m == 0){
                for(int i = 0; i<nums2.length;i++){
                    nums1[i] = nums2[i];
                }
                return;
            }
            if(nums1[index_1] <= nums2[index_2]){
                arr[0] = nums1[index_1];
                index_1++;
            }
            else{
                arr[0] = nums2[index_2];
                index_2++;
            }
            for(int i = 1;i<arr.length;i++){
                if(index_1 >= m){
                    arr[i] = nums2[index_2];
                    index_2++;
                }
                else if(index_2 >= n){
                    arr[i] = nums1[index_1];
                    index_1++;
                }
                else if(nums1[index_1]<=nums2[index_2])
                {
                    arr[i] = nums1[index_1];
                    index_1++;
                }
                else{
                    arr[i] = nums2[index_2];
                    index_2++;
                }

            }
            for(int j = 0;j<nums1.length;j++){
                nums1[j] = arr[j]; 
                // System.out.println(nums1[j]);
            }

        }
    
}
```

### 尾部遍历法

从数组后面往前填，直接将nums2填入nums1，然后对nums1进行排序
要记得活用**Arrays.sort()**方法哇

```java

    public void merge(int[] nums1, int m, int[] nums2, int n) {
        for (int i = 0; i != n; ++i) {
            nums1[m + i] = nums2[i];
        }
        Arrays.sort(nums1);
    }

```
## (142.环形链表)[https://leetcode.cn/problems/linked-list-cycle-ii/submissions/]


![](/img/posts/Leetcode/leetcode547.jpg)

假设从头节点走到入环节点需要a步， 每个环中有b个元素（也就是b步走完一个环）
核心在于确定当快慢指针相遇时的条件：
1. 当第一次slow和fast相遇：此时 **fast=2 slow**
    但由于fast比slow多走了nb
    可以得出 **fast = 2 nb ** **slow = nb**
但关键在于确定从从头节点走到入环点，由于从头节点走到入环点 = a， 走完这个环 = b，所以从此时slow再走a步就可以到达入环的节点，因为此时slow已经走了nb步了
2. 所以此时将fast放到head，从head走到入环点需要a步，而在环中的slow要再度走到入环点也需要a步，当fast和slow相遇的时候就是入环点了

![图解](/img/posts/Leetcode/leetcode547_explanation.jpg)

```java

/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        
        while(true){
            if(fast==null || fast.next==null) return null;
            
            slow = slow.next;
            fast = fast.next.next;
            if(fast == slow){
                break;
            }
            
        }
        fast = head;
        while(fast!=slow){
            fast = fast.next;
            slow = slow.next;
        }
        return fast;
    }
}
```
