---
title: Leetcode101-贪心算法
top: false
cover: false
author: DULULU oO
date: 2023-03-09 10:10:55
password:
summary:
tags: leetcode
categories: Leetcode101
---

## 贪心算法介绍

贪心算法是一种求解最优化问题的算法，它的核心思想是通过每一步选择局部最优解来达到全局最优解。

举一个简单的例子：假设你有一个背包，可以容纳重量为W的物品，现在有n个物品，每个物品的重量为wi，价值为vi。你想要在背包中装入尽可能多的价值，但是不能超过背包的容量。这个问题可以使用贪心算法来解决。

情景解释：
对于每个物品，我们可以计算其单位重量的价值，也就是vi/wi，然后按照这个值从大到小排序。然后我们依次将物品放入背包中，直到背包装满或者所有物品都放入为止。每次选择的物品都是当前剩余物品中单位重量价值最大的物品，这样可以保证我们在背包容量固定的情况下，放入的物品总价值最大。

具体实如下：
```python
def knapsack(W, wt, val):
    n = len(val)
    # 计算每个物品的单位重量价值
    unit_val = [(val[i]/wt[i], wt[i], val[i]) for i in range(n)]
    # 按照单位重量价值从大到小排序
    unit_val.sort(reverse=True)
    # 初始化当前背包重量和价值
    curr_wt, curr_val = 0, 0
    # 依次将物品放入背包中
    for i in range(n):
        if curr_wt + unit_val[i][1] <= W:
            curr_wt += unit_val[i][1]
            curr_val += unit_val[i][2]
        else:
            # 如果背包已经装满了，则跳出循环
            break
    return curr_val

```

在这个代码中，我们首先计算每个物品的**单位重量价值**，并按照这个值从大到小**排序**。然后我们依次将物品放入背包中，直到背包装满或者所有物品都放入为止。**每次选择的物品都是当前剩余物品中单位重量价值最大的物品**。最后返回背包中物品的总价值。

## [455.分发饼干](https://leetcode.cn/problems/assign-cookies/)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

 
```java
public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int count = 0;
        for(int i = 0, j = 0; i < g.length && j < s.length;){
            if(s[j] >= g[i]) {
                count++;
                i++;
                j++;
            }
            else j++;
        }
        return count;
```


## [135. 分发糖果](https://leetcode.cn/problems/candy)


n 个孩子站成一排。给你一个整数数组 ratings 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻两个孩子评分更高的孩子会获得更多的糖果。
请你给每个孩子分发糖果，计算并返回需要准备的 最少糖果数目 。



**思路：因为若是ratings= [1,3,5,2,1], candy = [1,2,3,2,1], 因为rating[2] = 5>rating[1]=3，所以ratings[2]=3**
**注意，该题需要左遍历一遍，再右遍历一遍**

```java

  public static int candy(int[] ratings) {
        int score = 0;
        int[] scoreArr = new int[ratings.length];
        int child = 1;
        scoreArr[0] = 1;
        for(; child < ratings.length; child++){
                if(compare(ratings[child], ratings[child-1]) > 0 ) {
                    scoreArr[child] = scoreArr[child - 1] + 1;
                }
                else{
                    scoreArr[child] = 1;
                }
        }

        score = scoreArr[child - 1];
        child -= 2;
        for (;child >= 0; child--){
                if(compare(ratings[child], ratings[child+1]) > 0) {
                    scoreArr[child] = scoreArr[child]>scoreArr[child+1]? scoreArr[child]:(scoreArr[child+1] + 1);
                }
                score += scoreArr[child];
        }

        return score;
    }

    public static int compare(int a, int b){
        if(a > b) return 1;
        else if(a == b) return 0;
        else return -1;
    }
```

## [435.无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals)

给定一个区间的集合 intervals ，其中 intervals[i] = [starti, endi] 。返回 需要移除区间的最小数量，使剩余区间互不重叠 。

示例 1:

输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。
示例 2:

输入: intervals = [ [1,2], [1,2], [1,2] ]
输出: 2
解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
示例 3:

输入: intervals = [ [1,2], [2,3] ]
输出: 0
解释: 你不需要移除任何区间，因为它们已经是无重叠的了。


### 解法一：按end快排

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        QuickSort(intervals, 0, intervals.length-1);
        int count = 0, end = intervals[0][0];
        for(int[] arr: intervals){
            if(arr[0] >= end){
                end = arr[1];
            }
            else{
                count++;
            }
        }
        return count;
    }

    public static void QuickSort(int[][] arr, int left, int right) {
        if (left >= right) return;
        int pivot = arr[right][1];
        int i = left, j = right;
        for(; i < j;){
            
            while(arr[i][1] <= pivot && i < j) i++;
            // System.out.println("i:"+i+" "+j);
            while(arr[j][1] >= pivot && j > i) j--;
            // System.out.println("j:"+i+" "+j);
            swap(arr, i, j);
        }
        swap(arr, i, right);
        QuickSort(arr, left, i-1);
        QuickSort(arr, i+1, right);

    }

    public static void swap(int[][] a, int i, int j) {
        int tmp_a = a[i][0];
        int tmp_b = a[i][1];
        a[i][0] = a[j][0];
        a[i][1] = a[j][1];
        a[j][0] = tmp_a;
        a[j][1] = tmp_b;
        
    }
}
```

### 解法二：直接调用Arrays.sort,不自己写快排

由于此时是比较每个数组的第1位元素，也就是a[1]和b[1]

因此直接将解法一中的
```java
QuickSort(intervals, 0, intervals.length-1);
```
换为
```java
Arrays.sort(intervals, (a, b) -> a[1] - b[1])
```
#### 代码
```java

    public int eraseOverlapIntervals(int[][] intervals) {
        // QuickSort(intervals, 0, intervals.length-1);
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
        int count = 0, end = intervals[0][0];
        for(int[] arr: intervals){
            if(arr[0] >= end){
                end = arr[1];
            }
            else{
                count++;
            }
        }
        return count;
    }
```
