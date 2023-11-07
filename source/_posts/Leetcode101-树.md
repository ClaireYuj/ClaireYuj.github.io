---
title: Leetcode101-树
top: false
cover: false
author: DULULU oO
date: 2023-04-10 00:04:36
password:
summary:
tags: leetcode
categories: Leetcode101
---
二叉树的遍历( traversing binary tree )是指从根结点出发，按照某种次序依次访问二叉树中所有结点，使得每个结点被访问一次且仅被访问一次。
1. 先序遍历
 先序遍历(PreOrder) 的操作过程如下：
 若二叉树为空，则什么也不做，否则，
 1)访问根结点;
 2)先序遍历左子树;
 3)先序遍历右子树
```cpp
void PreOrder(BiTree T){
	if(T != NULL){
		visit(T);	//访问根节点
		PreOrder(T->lchild);	//递归遍历左子树
		PreOrder(T->rchild);	//递归遍历右子树
	}
}

```

2. 中序遍历
 中序遍历( InOrder)的操作过程如下：
 若二叉树为空，则什么也不做，否则，
 1)中序遍历左子树;
 2)访问根结点;
 3)中序遍历右子树。
```cpp
void InOrder(BiTree T){
	if(T != NULL){
		InOrder(T->lchild);	//递归遍历左子树
		visit(T);	//访问根结点
		InOrder(T->rchild);	//递归遍历右子树
	}
}

```

3. 后序遍历

 后序遍历(PostOrder) 的操作过程如下：
 若二叉树为空，则什么也不做，否则，
 1)后序遍历左子树;
 2)后序遍历右子树;
 3)访问根结点。

```cpp
void PostOrder(BiTree T){
	if(T != NULL){
		PostOrder(T->lchild);	//递归遍历左子树
		PostOrder(T->rchild);	//递归遍历右子树
		visit(T);	//访问根结点
	}
}

```
二叉树可以通过**遍历序列**构造

由二叉树的**先序序列**和**中序序列**可以唯一地确定一棵二叉树。
在先序遍历序列中,第一个结点一定是二叉树的根结点;而在中序遍历中,根结点必然将中序序列分割成两个子序列,前一个子序列是根结点的左子树的中序序列,后一个子序列是根结点的右子树的中序序列。根据这两个子序列,在先序序列中找到对应的左子序列和右子序列。在先序序列中,左子序列的第一个结点是左子树的根结点,右子序列的第一个结点是右子树的根结点。
如此递归地进行下去,便能唯一地确定这棵二叉树
同理,由二叉树的后序序列和中序序列也可以唯一地确定一棵二叉树。
因为后序序列的最后一个结点就如同先序序列的第一个结点,可以将中序序列分割成两个子序列,然后采用类似的方法递归地进行划分,进而得到一棵二叉树。
由二叉树的层序序列和中序序列也可以唯一地确定一棵二叉树。
要注意的是,若只知道二叉树的先序序列和后序序列,则无法唯一确定一棵二叉树。
例如,求先序序列( ABCDEFGHI)和中序序列(BCAEDGHFI）所确定的二叉树
首先,由先序序列可知A为二叉树的根结点。中序序列中A之前的BC为左子树的中序序列,EDGHFI为右子树的中序序列。然后由先序序列可知B是左子树的根结点,D是右子树的根结点。以此类推,就能将剩下的结点继续分解下去,最后得到的二叉树如图所示。
![](/img/posts/Leetcode/binarytree.jpg)

## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
![](/img/posts/Leetcode/leetcode94.jpg)

虽然给的是要返回List，但可以自己写一个inorder来进行递归
```java
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        inorder(root, list);
        return list;
        
    }

    public void inorder(TreeNode root, List<Integer> list){
        if(root == null) return;
        inorder(root.left, list); //左子节点递归
        list.add(root.val); // 根节点
        inorder(root.right, list); //右子节点
    }

```

## [98.验证二叉搜索数](https://leetcode.cn/problems/validate-binary-search-tree/)

![](/img/posts/Leetcode/leetcode98.jpg)


注意 **root.left<root.left.right<root**，因此可以给每个值设置一个max和min，每次迭代时更新：
![因为4<5，但4是5的右子树中的节点，所有为false](/img/posts/Leetcode/leetcode98_false.jpg)
1. 若该节点为左子节点，max更新为root.val，min保持（因为该节点可能是上上一个节点的右节点，保存上上个节点的值为最小值）
2. 若为右节点，则反之
```java

class Solution {
    public boolean isValidBST(TreeNode root) {
        
        long min = (long) (Math.pow(-2, 31)-2); //定义边界，注意等于号！！
        long max = (long) (Math.pow(2, 31)+2);
        
         return judge(root, min,max);
    }
    public boolean judge(TreeNode root, long min, long max){
        if(root == null) return true;
        if((root.left!=null&&((root.left.val >= root.val||root.left.val <= min) ))||(root.right!=null && (root.right.val <= root.val || root.right.val >= max))) return false; // 当左节点不为空，且左子节点大于根节点或小于最小值 || 当右节点不为空，且右节点大于最大值或小于根节点时 为false


        return judge(root.left, min, root.val) && judge(root.right, root.val, max);
    }
}
```

## [96.不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数

![](/img/posts/Leetcode/leetcode96.jpg)


是用数学方法进行推导的，以[1,2,3,4,5]为例
**dp[i]表示1~i形成的二叉搜索树的数量**

![](/img/posts/Leetcode/leetcode96_1.jpg)

当root=3时，左子树为[1,2] 右子树为[4,5],因为**对于二叉搜索树而言，确定了序列，也就确定了这棵树**，所有[1,2] = [4,5] = dp[2], **[1,2][3][4,5] = dp[2] \* dp[2] = dp[i-1]*dp[n-i]**
由于[1,2,3,4,5]可以分别以1~5为根，因此公式为
<center>dp[n] = dp[n-1]*dp[0] + dp[n-2]*dp[1]....+dp[0]*dp[n-1]


```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2;i<n+1;i++){
            for(int j = 1;j<i+1;j++){
                dp[i] += dp[j - 1] * dp[i-j];
            }
        }
        return dp[n];
    }
}
```