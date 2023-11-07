---
title: Leetcode-图论
top: false
cover: false
author: DULULU oO
date: 2021-08-17 00:58:50
password:
summary:
tags: leetcode
categories: 数据结构
---

# 无向图

定义：**无向图是由一组顶点（vertex）和一组能够将两个顶点相连的边（edge）组成的**
在无向图中，允许出现1.自环，即一条连接一个顶点和其自身的边，2. 连接同一对顶点的两条边称为平行边

## 261. 以图判树

给定从 0 到 n-1 标号的 n 个结点，和一个无向边列表（每条边以结点对来表示），请编写一个函数用来判断这些边是否能够形成一个合法有效的树结构。

![Leetcode261图示](/img/posts/Leetcode/leetcode261.jpg)

### 代码
```java
    class Solution {
        //判断无向图是否有换，以及是否为单连通分量
        //当只有n-1条边，且连通数为1时，符合条件
        public boolean validTree(int n, int[][] edges) {
            if(edges.length+1 != n) //只应该有n-1条边
                return false;
            int[] par = new int[n];
            int par1 = 0,par2 = 0;
            for(int i = 0;i < n;i++)
                par[i]=i; //并查集，每个点的父节点是自己
            for(int i = 0;i < edges.length;i++){
                par1 = edges[i][0];
                par2 = edges[i][1];
                /*不断向上寻找得到par1和par2的祖先节点*/
                while(par1 != par[par1])
                    par1 = par[par1]; 
                while(par2 != par[par2])
                    par2 = par[par2];
                /*par2和par1不属于一个并查集，将par2并入par1，此时并查集个数n--*/
                if(par1 != par2){
                    par[par2] = par1;
                    n--;
                }
            }
            return n == 1;
            
        }
    }
```


## 323. 无向图中连通分量的数目

你有一个包含 n 个节点的图。给定一个整数 n 和一个数组 edges ，其中 edges[i] = [ai, bi] 表示图中 ai 和 bi 之间有一条边。

![Leetcode323图示](/img/posts/Leetcode/leetcode323.jpg)

### c++ vector容器进行DFS 深度优先
```c++
class Solution {
public:
	int countComponents(int n, vector<vector<int>>& edges) {
		vector<int>a(n, 0);
		vector<vector<int>>arr(n, a);
		for (auto it : edges) {
			arr[it[0]][it[1]] = 1;
			arr[it[1]][it[0]] = 1;
		}
		int res = 0;
		vector<int>mark(n, 0);
		for (int i = 0; i < n; i++) {
			if (mark[i] == 0) {
				DFS(arr, mark, i);
				res++;
			}
		}
		return res;
		
	}
private:
	void DFS(vector<vector<int>>&arr, vector<int>& mark,int b) {
			mark[b] = 1;
			for (int i = 0; i < arr.size(); i++) {
				if (arr[i][b] == 1&&mark[i] == 0) {
					DFS(arr, mark,i);
				}
			}
			mark[b] = 2;
	}
};
```
### java 二维数组进行dfs
```java
class Solution {
    public int countComponents(int n, int[][] edges){
        int[][] map = new int[n][n];
        for(int[] edge : edges) {
            map[edge[0]][edge[1]] = 1;
            map[edge[1]][edge[0]] = 1;
        }
        int res = 0;
        int[] mark = new int[n];
        for(int i = 0; i < n; i ++) {
            if(mark[i] == 0) {
                dfs(map, mark, i);
                res ++;
            }
        }
        return res;
    }
    
    public void dfs(int[][] map, int[] mark, int i) {
        mark[i] = 1;
        for(int j = 0; j < map.length; j ++) {
            if(map[i][j] == 1 && mark[j] == 0) {
                dfs(map,mark,j);
            }
        }
    }
}
```

##  547.省份数量

有n个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

![Leetcode547](/img/posts/Leetcode/leetcode547.jpg)


### 解法：DFS 深度优先

关键在于：无向图最小连通数：并查集

```java
class Solution {
    public int findCircleNum(int[][] M) {
        int ans = 0, N = M.length;
        int[] mark = new int[N];
        for(int i = 0; i < N; i++){
            if(mark[i] == 0){
                ans++;
                DFS(M,mark,i,N);
            }
        }
        return ans;
    }
    
    public void DFS(int[][] M, int[] mark, int si, int N){
        mark[si] = 1;
        for(int j = 0; j < N; j++){
            if(M[si][j] == 1 && mark[j] == 0){
                DFS(M,mark,j,N);
            }
        }
    }
}
```

