---
title: Leetcode101-搜索算法
top: false
cover: false
author: DULULU oO
date: 2023-03-26 00:09:11
password:
summary: 
tags: leetcode
categories: Leetcode101
---


DFS:深度优先搜索 (DFS)：DFS是通过**递归**搜索来实现的，它沿着树的深度搜索，直到找到目标节点或到达末端。
步骤，从初始节点->判断是否满足进入下一个节点的条件->进入下一个节点->进行遍历操作->在该节点继续dfs

```java
void DFS(Node node) {
    if (node == null) {
        return;
    }
    visit(node);
    node.visited = true;
    for (Node n : node.adjacent) {
        if (n.visited == false) {
            DFS(n);
        }
    }
}

```

而广度有限BFS:是一种图形搜索算法，它在图中沿着宽度遍历，先找到所有与起点节点相邻的节点，然后再找到与它们相邻的节点，以此类推。广度优先中常使用**队列**存储

```java
void BFS(Node node) {
    Queue<Node> queue = new LinkedList<Node>();
    node.visited = true;
    queue.add(node);
    while (!queue.isEmpty()) {
        Node element = queue.remove();
        visit(element);
        for (Node n : element.adjacent) {
            if (n.visited == false) {
                n.visited = true;
                queue.add(n);
            }
        }
    }
}

```


## [695.岛屿问题](https://leetcode.cn/problems/max-area-of-island/)

给你一个大小为 m x n 的二进制矩阵 grid 。

岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在 水平或者竖直的四个方向上 相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

岛屿的面积是岛上值为 1 的单元格的数目。

计算并返回 grid 中最大的岛屿面积。如果没有岛屿，则返回面积为 0

![示例](/img/posts/Leetcode/leetcode695.jpg)

用dfs
```java

class Solution {
    //可以看图理解，在加上记住这个模板。
    public int maxAreaOfIsland(int[][] grid) {
        //定义一个表示岛屿的面积
        int max = 0;
        //这两个for循环是来遍历整张二维格上的所有陆地的。
        //i 表示行，j表示列
        for(int i = 0;i<grid.length;i++){
            for(int j = 0; j<grid[0].length;j++){
                //陆地的格
                if(grid[i][j]==1){
                    //取出最大的面积
                    max = Math.max(max,dfs(grid,i,j));
                }  
            }
        }
        //返回最大的陆地面积
        return max;
    }
    public int  dfs(int[][] grid,int i,int j){
        //当超出岛屿边界（上下左右）的时候，就直接退出，特别要加上当遍历到海洋的时候也要退出，
        if(i<0||j<0 || i>=grid.length || j>= grid[0].length|| grid[i][j]==0) return 0;
       //定义一个变量表示岛屿的面积，就是包含几个陆地
        int sum = 1;
        //将陆地改为海洋，防止重复陆地重复遍历。
        grid[i][j] =0;
        //遍历上方元素，每遍历一次陆地就加一
        sum += dfs(grid,i+1,j);
        //遍历下方元素
        sum +=dfs(grid,i-1,j);
        //遍历右边元素
        sum +=dfs(grid,i,j+1);
        //遍历左边元素
        sum += dfs(grid,i,j-1);
        return sum;
    }
}
```

## [547.省份数量](https://leetcode.cn/problems/number-of-provinces/) 

有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

![示例](/img/posts/Leetcode/leetcode547.jpg)

```java

class Solution {
    public static int findCircleNum(int[][] isConnected) {
        int province[] = new int[isConnected.length]; //利用province来记录
        int count = 1;
        // province[0] = 1;
        for(int i = 0;i<isConnected.length;i++){
            if(province[i] == 0) { // 只有当province为0的时候才会进入新的一个省份，需要重新进行dfs
                province[i] = count;
                count++;
            
                for(int j = 0; j<isConnected.length; j++){
                    if(isConnected[i][j] == 1 && province[j] == 0){
                        province[j] = province[i];
                        dfs(isConnected, j, province);
                    }
                }
            }
        }   
        return count-1;
    }

    public static void dfs(int[][]a, int j, int province[]){
        if(j<a.length){
            for(int k = 0;k<a.length;k++){
                if(a[j][k]==1 && province[k] == 0){
                    province[k] = province[j]; // 每次dfs将province[k]设成现在省份的index
                    dfs(a, k ,province);
                }
            }
        }

    }
}
```