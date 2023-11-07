---
title: JAVA常见容器
top: false
cover: false
author: DULULU oO
date: 2021-10-14 22:51:54
password:
summary:
tags: JAVA
categories: 数据结构
---


## Iterator

Iterable被Collection所继承。它只有一个方法： Iterator<T> iterator() 

迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为**“轻量级”**对象，因为创建它的代价小。

Java中的Iterator功能比较简单，并且只能单向移动：
　　(1) 使用方法iterator()要求容器返回一个Iterator。第一次调用Iterator的next()方法时，它返回序列的第一个元素。注意：iterator()方法是java.lang.Iterable接口,被Collection继承。
　　(2) 使用next()获得序列中的下一个元素。
　　(3) 使用hasNext()检查序列中是否还有元素。
　　(4) 使用remove()将迭代器新返回的元素删除。

Iterator是Java迭代器最简单的实现，为List设计的ListIterator具有更多的功能，它可以从两个方向遍历List，也可以从List中插入和删除元素。

## Collection

一般不会直接实现collection接口，而是实现它的子类

### List

***有序的，可重复的***
List是有序的 collection子接口。此接口的用户可以对列表中每个元素的插入位置进行精确地控制。用户可以根据元素的整数索引（在列表中的位置）访问元素，并搜索列表中的元素。

用户插入的顺序或者指定的位置就是元素插入的位置。它与Set不同，List允许插入重复的值。

List 接口提供了特殊的迭代器，称为 ListIterator，除了允许 Iterator 接口提供的正常操作外，该迭代器还允许元素插入和替换，以及双向访问。还提供了一个方法(如下)来获取从列表中指定位置开始的列表迭代器。

ListIterator <E> listIterator(int index)
返回列表中元素的列表迭代器（按适当顺序），从列表的指定位置开始

List 接口提供了两种搜索指定对象的方法。从性能的观点来看，应该小心使用这些方法。在很多实现中，它们将执行高开销的线性搜索。

List 接口提供了两种在列表的任意位置高效插入和移除多个元素的方法。

### Set

***无序的，不可重复的***

java的集合和数学的集合一样，集合的无序性，确定性，单一性。所以可以很好的理解，Set是无序、不可重复的。同时，如果有多个null，则不满足单一性了，Set只能有一个null。
Set判断两个对象相同不是使用"=="运算符，**而是根据equals方法**。——因为Set的这个制约，在使用Set集合的时候，应该注意两点：
为Set集合里的元素的实现类实现一个有效的equals(Object)方法；
对Set的构造函数，传入的Collection参数不能包含重复的元素。

## Queue

***有序的，可重复的***
用于模拟“队列”数据结构（FIFO）。新插入的元素放在队尾，队头存放着保存时间最长的元素。

Queue的子类、子接口
1.1） PriorityQueue—— 优先队列（类）
其实它并没有按照插入的顺序来存放元素，而是按照队列中某个属性的大小来排列的。故而叫优先队列。

1.2） Deque——双端队列（接口）
1.2.1）ArrayDeque（类）
基于数组的双端队列，类似于ArrayList有一个Object[] 数组。
1.2.2）LinkedList （类）

## Map

Map不是collection的子接口或者实现类，Map是一个接口。

Map用于保存具有“映射关系”的数据。每个Entry都持有**键-值**两个对象。其中，Value可能重复，但是Key不允许重复（和Set类似）。

Map可以有多个Value为null，但是只能有一个Key为null
