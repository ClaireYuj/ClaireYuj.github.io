---
title: leetcode101--二分查找
top: false
cover: false
author: DULULU oO
date: 2023-04-12 14:50:29
password:
summary:
tags: leetcode
categories: Leetcode101
---

## [704.二分查找](https://leetcode.cn/problems/binary-search/) 


给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

注意二分查找主要是边界的确定 也就是判断是使用while(left < right)还是whie(left <= right),这个时候就要判断时左闭右闭区间还是左闭右开区间

### 左闭右闭
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length-1,mid = (left+right) / 2;
        while(left<=right){
            if(nums[mid] == target) return mid;
            else if(nums[mid] > target) {
                right = mid-1;
                mid = (left+right)/2;
            }
            else{
                left = mid + 1;
                mid = (left+right)/2;
            }
        }
        return -1;
    }
}
```

### 左闭右开

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length,mid = (left+right) / 2; //right初值改变
        while(left<right){ //while判断条件改变
            if(nums[mid] == target) return mid;
            else if(nums[mid] > target) {
                right = mid; //right改变，因为 右开， 不需要mid-1
                mid = (left+right)/2;
            }
            else{
                left = mid + 1; // left不变，因为左闭
                mid = (left+right)/2;
            }
        }
        return -1;
    }
}

```