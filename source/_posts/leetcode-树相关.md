---
title: Leetcode--树相关
top: false
cover: false
author: DULULU oO
date: 2021-10-17 00:27:39
password:
summary:
tags: leetcode
categories: 数据结构
---

## 树的特征

树是一幅无环连通图。互不相连的树组成的集合称为森林。连通图的生成树是它的一幅子图，它含有图中的所有顶点且是一颗树。图的生成树森林是它的所有连通子图的生成树的集合。

有多个子节点，层层递归（一般是二叉树）
二叉树的定义如下
```java
 public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```

## 剑指Offer25. 树的子结构

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。
![](/img/posts/Leetcode/offer26.jpg)
前序遍历，从A树和B树相同的结点开始，B树的左子树 = A树的左子树；A树的右子树 = B树的右子树
结束条件==》B树符合A树左子树条件||A树 = null


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A==null||B==null) return false;
        return travel(A,B)||isSubStructure(A.left,B)||isSubStructure(A.right,B); // 从该节点开始遍历||判断从A的左节点开始遍历，B是不是A的左子树的子结构|| 判断B是不是A的右子树的子结构

    }

    public boolean travel(TreeNode A, TreeNode B){
        if(B == null) return true; // 表示在B为null前A与B 的val一直相等
        else if(A == null && B != null) return false;
        else if(A.val != B.val) return false;
        return travel(A.left, B.left)&&travel(A.right, B.right); // 分辨遍历左右子树
    }
}

```


## Offer27.二叉树的镜像

[请完成一个函数，输入一个二叉树，该函数输出它的镜像](https://leetcode.cn/problems/invert-binary-tree/)
![](/img/posts/Leetcode/offer27.jpg)

- 思路： 从根的叶结点开始， 一个一个交换左右子树，一直到为null

```java
    class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null ) return null;
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        invertTree(root.right);
        invertTree(root.left);
        return root;
    }
}


```

## Offer28.对称的二叉树

![](https://leetcode.cn/leetbook/read/illustration-of-algorithm/5d412v/)
依旧是递归，不过考虑到判断对称，要求：
左子树的左子树等于右子树的右子树，右子树的左子树=左子树的右子树，依次递归

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        TreeNode left = root.left;
        TreeNode right = root.right;
        if(left!=null&&right!=null&&right.val==left.val){
            return travel(left,right);
        }
        else if(left == null &&right==null)return true;
        else return false;

    }

    public boolean travel(TreeNode left, TreeNode right){
        if(left==null&&right==null) return true;
        if(left!=null&&right!=null&&left.val!=right.val) return false;
        else if(left==null||right==null) return false;
        return travel(left.left,right.right)&&travel(right.left,left.right);
    }
}
```

## 94. 二叉树的中序遍历
![Leetcode94](/img/posts/Leetcode/leetcode94.jpg)

### 递归 

中序遍历 左->中-右

```jAVA
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> tree = new ArrayList<Integer>();
        inOrder(root,tree);
        return tree;
        
    }

    public void inOrder(TreeNode root,List<Integer> tree){
        if(root==null){
            return;
        }
        inOrder(root.left,tree);
        tree.add(root.val);
        inOrder(root.right,tree);
    }
}
```

## Offer22. 广度优先算法

[从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。](https://leetcode.cn/leetbook/read/illustration-of-algorithm/9ackoe/)

### 代码
```java
**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
        
        public int[] levelOrder(TreeNode root) {

            int[] arr = this.bfs(root);
            return arr;
        }
        
        // 广度优先要用queue
        public int[] bfs(TreeNode root){
            Queue<TreeNode> queue = new LinkedList<>();
            LinkedList<Integer> ans =  new LinkedList<>(); //用来确定int[] arr的大小
            if(root != null)
                queue.offer(root); //先将root入队列

            for(;!queue.isEmpty();){
                TreeNode treeNode = queue.remove(); // 将根节点出列
                ans.add(treeNode.val);
                // 两个if实现了广度有限中将同层次的叶节点加入队列
                if(treeNode.left!=null){
                    queue.offer(treeNode.left);
                }
                if (treeNode.right != null){
                    queue.offer(treeNode.right);
                }
            }
            int[] arr = new int[ans.size()];
            for (int i = 0; i < arr.length;i++){
                arr[i] = ans.get(i);
            }

            return arr;
        }

}
```



## 95.不同的二叉搜索树

给定一个整数 n，生成所有由 1 ... n 为节点所组成的二叉搜索树。
（不只返回数字）

### 解法一:DFS 深度优先

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if(n == 0) return new ArrayList<TreeNode>();
        return helper(1, n); 
    } 

    List<TreeNode> helper(int start, int end){
        List<TreeNode> list = new ArrayList<>();
        if(start > end) list.add(null);
        
        for(int i = start; i <= end; i++){
            List<TreeNode> left = helper(start, i - 1);
            List<TreeNode> right = helper(i + 1, end);
            
            for(int j = 0; j < left.size(); j++){
                for(int k = 0; k < right.size(); k++){
                    TreeNode root = new TreeNode(i);
                    root.left = left.get(j);
                    root.right = right.get(k);
                    list.add(root);
                }
            }
        }
        return list;  
    }
}  
```

### 解法二：DP 动态规划

```Java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i = 2; i < n + 1; i++)
            for(int j = 1; j < i + 1; j++) 
                dp[i] += dp[j-1] * dp[i-j];
        
        return dp[n];
    }
}
```

## 261. 以图判树

### 题目要求
给定从 0 到 n-1 标号的 n 个结点，和一个无向边列表（每条边以结点对来表示），请编写一个函数用来判断这些边是否能够形成一个合法有效的树结构。

### 代码
```Java
class Solution {
    //判断无向图是否有换，以及是否为单连通分量
    //当只有n-1条边，且连通数为1时，符合条件
    public boolean validTree(int n, int[][] edges) {
        if(edges.length+1 != n) //只应该有n-1条边
            return false;
        int[] par = new int[n];
        int par1 = 0,par2 = 0;
        for(int i=0;i<n;i++)
            par[i]=i; //并查集，每个点的父节点是自己
        for(int i=0;i<edges.length;i++){
            par1 = edges[i][0];
            par2 = edges[i][1];
            //不断向上寻找得到par1和par2的祖先节点
            while(par1 != par[par1])
                par1 = par[par1]; 
            while(par2 != par[par2])
                par2 = par[par2];
            //par2和par1不属于一个并查集，将par2并入par1，此时并查集个数n--
            if(par1 != par2){
                par[par2] = par1;
                n--;
            }
        }
        return n == 1;
        
    }
}
```

## 515. 在二叉树的每一行中找到最大的值。

### 解法：BFS 广度优先
```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> rlist = new ArrayList<>();
        if(root==null) return rlist;
        
        List<TreeNode> nodeList = new LinkedList<>();
        nodeList.add(root);
        while(!nodeList.isEmpty()){
            int size = nodeList.size();
            int max = nodeList.get(0).val;
            for(int i = 0;i< size;i++){
                TreeNode node = nodeList.remove(0);
                max = Math.max(node.val,max);
                if(node.left!=null) nodeList.add(node.left);
                if(node.right!=null) nodeList.add(node.right);

            }
            rlist.add(max);
        }
        return rlist;
        
    }
}
```

