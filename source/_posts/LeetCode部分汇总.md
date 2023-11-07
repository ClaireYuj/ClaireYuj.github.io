---
title: Leetcode部分汇总
top: true
cover: false
date: 2022-08-02 15:01:26
password:
summary:
tags: leetcode
categories: 数据结构
---

# LeetCode

---
## 96. 不同的二叉搜索树:DP
给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
### 思路
DP、卡特兰公式
### 代码
```java
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
## 95.不同的二叉搜索树2:DFS
给定一个整数 n，生成所有由 1 ... n 为节点所组成的二叉搜索树。
（不只返回数字）
### 思路
DFS
### 代码
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
## 516. 最长回文子序列:DP
给定一个字符串s，找到其中最长的回文子序列（的长度）。
注意：子序列可以不连续的，可以跳过某些单词，子串是必须连续的
### 思路
DP
### 代码
```java

```
## 714. 买卖股票的最佳时机含手续费:DP
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int cash = 0, hold = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            cash = Math.max(cash, hold + prices[i] - fee);
            hold = Math.max(hold, cash - prices[i]);
        }
        return cash;
    }
}
```
## 139. 单词拆分:DP/BST
### DP
```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```
### BST
```java
Javapublic class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        Queue<Integer> queue = new LinkedList<>();
        int[] visited = new int[s.length()];
        queue.add(0);
        while (!queue.isEmpty()) {
            int start = queue.remove();
            if (visited[start] == 0) {
                for (int end = start + 1; end <= s.length(); end++) {
                    if (wordDictSet.contains(s.substring(start, end))) {
                        queue.add(end);
                        if (end == s.length()) {
                            return true;
                        }
                    }
                }
                visited[start] = 1;
            }
        }
        return false;
    }
}
```
## 516. 最长回文子序列:DP
### DP 步骤版
```java
class Solution {
    public int longestPalindromeSubseq2nd(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[] pre = new int[n];
        int[] cur = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            cur[i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    cur[j] = pre[j - 1] + 2;
                } else {
                    cur[j] = Math.max(pre[j], cur[j - 1]);
                }
            }
            int[] temp = pre;
            pre = cur;
            cur = temp;
        }
        return pre[n - 1];
    }
}
```
## 300.最长上升子序列:DP、二分法插入
### 代码
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int len = nums.length;
        if (len < 2) {
            return len;
        }
        // 这个数组实际上的长度，就是最后所求
        int[] tail = new int[len];
        int end = 0;
        tail[0] = nums[0];
        for (int i = 1; i < len; i++) {
            if (nums[i] > tail[end]) {
                end++;
                tail[end] = nums[i];
            } else {
                // 使用二分搜索法来做这件事情，二分法实现nlogn的时间复杂度
                // 修改目前tail中比target小的最大数为target
                int left = 0;
                int right = end;
                int target = nums[i];
                while (left < right) {
                    int mid = (right + left) / 2;
                    //int mid = left + (right - left) / 2;
                    if (tail[mid] < target) {
                        // 只要比目标值要小，要找的位置就至少是当前位置 + 1
                        left = mid + 1;
                    } else {
                        assert tail[mid] >= target;
                        // 大于目标值，不能盲目向前走，因为向前走很可能，值会变得比目标值小
                        right = mid;
                    }
                }
                tail[right] = target;
            }
        }
        return end + 1;
    }
}
```
## 494. 目标和:DP+背包问题
```java
class Solution {
     /**
     * sum(P) 前面符号为+的集合；sum(N) 前面符号为减号的集合
     * 所以题目可以转化为
     * sum(P) - sum(N) = target 
     * => sum(nums) + sum(P) - sum(N) = target + sum(nums)
     * => 2 * sum(P) = target + sum(nums) 
     * => sum(P) = (target + sum(nums)) / 2
     * 因此题目转化为01背包，也就是能组合成容量为sum(P)的方式有多少种
     */
    public static int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum < S || (sum + S) % 2 == 1) {
            return 0;
        }
        int w = (sum + S) / 2;
        int[] dp = new int[w + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int j = w; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        return dp[w];
    }
}
```
## 638. 大礼包:DP
```java
class Solution {
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        //统计不使用大礼包的总价
        int noSpecial = 0;
        for(int i = 0;i<needs.size();i++){
            noSpecial += price.get(i) * needs.get(i);
        }
        int res = noSpecial;
        //遍历每一个大礼包
        for(List<Integer> sp : special){
            //当前大礼包超过购买数量，跳过
            if(check(sp,needs)){
                //使用当前大礼包后，还有多少剩下的
                List<Integer> newNeeds = new ArrayList<>();
                for(Integer i = 0;i<sp.size() - 1;i++){
                    newNeeds.add(needs.get(i) - sp.get(i));
                }
                //剩下的购买数量递归调用本方法，获取最低价格
                int left = shoppingOffers(price,special,newNeeds);
                //使用当前大礼包和不使用相比，选价格最低的
                res = Math.min(res,left + sp.get(sp.size() - 1));
            }
        }
        return res;
    }
    private boolean check(List<Integer> special,List<Integer> needs){
        for(int i = 0;i<needs.size();i++){
            if(special.get(i) > needs.get(i)){
                return false;
            }
        }
        return true;
    }
}
```
## 651. 4键键盘:DP+贪心算法
复制粘贴还是直接输入，得到最长字符串
```java
class Solution {
    public int maxA(int N) {
        if(N<=5) return N;
        int[] arr = new int[N+1];
        arr[0] = 0;
        for(int i=1;i<=N;i++){
            arr[i] = arr[i-1] + 1;
            for(int j=2;j+1<i;j++){
                arr[i] = getMax(arr[i],arr[j-1]*(i-j));
            }
        }
        return arr[N];
    }
    public int getMax(int a,int b){
        return a>b ? a:b;
    }
}
```
## 983. 最低票价:DP
买火车票：1天、7天、30天
```java
class Solution {
    int[] days, costs;
    Integer[] memo;
    int[] durations = new int[]{1, 7, 30};

    public int mincostTickets(int[] days, int[] costs) {
        this.days = days;
        this.costs = costs;
        memo = new Integer[days.length];

        return dp(0);
    }
    public int dp(int i) {
        if (i >= days.length)
            return 0;
        if (memo[i] != null)
            return memo[i];

        int ans = Integer.MAX_VALUE;
        int j = i;
        for (int k = 0; k < 3; ++k) {
            while (j < days.length && days[j] < days[i] + durations[k])
                j++;
            ans = Math.min(ans, dp(j) + costs[k]);
        }

        memo[i] = ans;
        return ans;
    }
}
```
## 323. 无向图中连通分量的数目:DFS
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
## 529. 扫雷游戏:BFS/DFS
```java
class Solution {
   static public char[][] updateBoard(char[][] board, int[] click) {
       
   	return visit(board,click[0],click[1]);	
   }
	
  static public char[][] visit(char[][] board, int x,int y){
	   if(board[x][y] == 'M') {
		   board[x][y] = 'X';
		   return board;
	   }
   	int count = 0;
       for(int i =x-1;i<=x+1;i++)
          for(int j =y-1; j<= y+1;j++){
        	  if(i>=0 && i < board.length && j>=0 && j<board[0].length)
              if(board[i][j] == 'M')
                   count++;
           }
   	if(count != 0 ) {
   		board[x][y] = (char)(count+'0');
   	}
   	else {
   		board[x][y] = 'B';
   	    for(int i =x-1;i<=x+1;i++)
               for(int j =y-1; j<= y+1;j++){
            	   if(i>=0 && i < board.length && j>=0 && j<board[0].length)
                   if(board[i][j] == 'E')
                       visit(board,i,j);
               }

   	}
   	
   	return board;
   }
}
```
## 127. 单词接龙：BFS
```java
import javafx.util.Pair;

class Solution {
  public int ladderLength(String beginWord, String endWord, List<String> wordList) {

    // Since all words are of same length.
    int L = beginWord.length();

    // Dictionary to hold combination of words that can be formed,
    // from any given word. By changing one letter at a time.
    HashMap<String, ArrayList<String>> allComboDict = new HashMap<String, ArrayList<String>>();

    wordList.forEach(
        word -> {
          for (int i = 0; i < L; i++) {
            // Key is the generic word
            // Value is a list of words which have the same intermediate generic word.
            String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);
            ArrayList<String> transformations =
                allComboDict.getOrDefault(newWord, new ArrayList<String>());
            transformations.add(word);
            allComboDict.put(newWord, transformations);
          }
        });

    // Queue for BFS
    Queue<Pair<String, Integer>> Q = new LinkedList<Pair<String, Integer>>();
    Q.add(new Pair(beginWord, 1));

    // Visited to make sure we don't repeat processing same word.
    HashMap<String, Boolean> visited = new HashMap<String, Boolean>();
    visited.put(beginWord, true);

    while (!Q.isEmpty()) {
      Pair<String, Integer> node = Q.remove();
      String word = node.getKey();
      int level = node.getValue();
      for (int i = 0; i < L; i++) {

        // Intermediate words for current word
        String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);

        // Next states are all the words which share the same intermediate state.
        for (String adjacentWord : allComboDict.getOrDefault(newWord, new ArrayList<String>())) {
          // If at any point if we find what we are looking for
          // i.e. the end word - we can return with the answer.
          if (adjacentWord.equals(endWord)) {
            return level + 1;
          }
          // Otherwise, add it to the BFS Queue. Also mark it visited
          if (!visited.containsKey(adjacentWord)) {
            visited.put(adjacentWord, true);
            Q.add(new Pair(adjacentWord, level + 1));
          }
        }
      }
    }

    return 0;
  }
}
```
双向广度优先搜索
```java
import javafx.util.Pair;

class Solution {

  private int L;
  private HashMap<String, ArrayList<String>> allComboDict;

  Solution() {
    this.L = 0;

    // Dictionary to hold combination of words that can be formed,
    // from any given word. By changing one letter at a time.
    this.allComboDict = new HashMap<String, ArrayList<String>>();
  }

  private int visitWordNode(
      Queue<Pair<String, Integer>> Q,
      HashMap<String, Integer> visited,
      HashMap<String, Integer> othersVisited) {
    Pair<String, Integer> node = Q.remove();
    String word = node.getKey();
    int level = node.getValue();

    for (int i = 0; i < this.L; i++) {

      // Intermediate words for current word
      String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);

      // Next states are all the words which share the same intermediate state.
      for (String adjacentWord : this.allComboDict.getOrDefault(newWord, new ArrayList<String>())) {
        // If at any point if we find what we are looking for
        // i.e. the end word - we can return with the answer.
        if (othersVisited.containsKey(adjacentWord)) {
          return level + othersVisited.get(adjacentWord);
        }

        if (!visited.containsKey(adjacentWord)) {

          // Save the level as the value of the dictionary, to save number of hops.
          visited.put(adjacentWord, level + 1);
          Q.add(new Pair(adjacentWord, level + 1));
        }
      }
    }
    return -1;
  }

  public int ladderLength(String beginWord, String endWord, List<String> wordList) {

    if (!wordList.contains(endWord)) {
      return 0;
    }

    // Since all words are of same length.
    this.L = beginWord.length();

    wordList.forEach(
        word -> {
          for (int i = 0; i < L; i++) {
            // Key is the generic word
            // Value is a list of words which have the same intermediate generic word.
            String newWord = word.substring(0, i) + '*' + word.substring(i + 1, L);
            ArrayList<String> transformations =
                this.allComboDict.getOrDefault(newWord, new ArrayList<String>());
            transformations.add(word);
            this.allComboDict.put(newWord, transformations);
          }
        });

    // Queues for birdirectional BFS
    // BFS starting from beginWord
    Queue<Pair<String, Integer>> Q_begin = new LinkedList<Pair<String, Integer>>();
    // BFS starting from endWord
    Queue<Pair<String, Integer>> Q_end = new LinkedList<Pair<String, Integer>>();
    Q_begin.add(new Pair(beginWord, 1));
    Q_end.add(new Pair(endWord, 1));

    // Visited to make sure we don't repeat processing same word.
    HashMap<String, Integer> visitedBegin = new HashMap<String, Integer>();
    HashMap<String, Integer> visitedEnd = new HashMap<String, Integer>();
    visitedBegin.put(beginWord, 1);
    visitedEnd.put(endWord, 1);

    while (!Q_begin.isEmpty() && !Q_end.isEmpty()) {

      // One hop from begin word
      int ans = visitWordNode(Q_begin, visitedBegin, visitedEnd);
      if (ans > -1) {
        return ans;
      }

      // One hop from end word
      ans = visitWordNode(Q_end, visitedEnd, visitedBegin);
      if (ans > -1) {
        return ans;
      }
    }

    return 0;
  }
}
```
## 139. 单词拆分：BFS/DP
BFS
```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        Queue<Integer> queue = new LinkedList<>();
        int[] visited = new int[s.length()];
        queue.add(0);
        while (!queue.isEmpty()) {
            int start = queue.remove();
            if (visited[start] == 0) {
                for (int end = start + 1; end <= s.length(); end++) {
                    if (wordDictSet.contains(s.substring(start, end))) {
                        queue.add(end);
                        if (end == s.length()) {
                            return true;
                        }
                    }
                }
                visited[start] = 1;
            }
        }
        return false;
    }
}
```
DP
```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```
## 279. 完全平方数:BST
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。
```java
static class Node {
        int val;
        int step;

        public Node(int val, int step) {
            this.val = val;
            this.step = step;
        }
    }

    // 将问题转化成图论
    // 该算法在往队列里面添加节点的时候会 add 很多重复的节点，导致超时，
    // 优化办法是，加入 visited 数组，检查要 add 的数据是否已经出现过了，防止数据重复出现，从而影响图的遍历
    // 同时优化：num - i * i 表达式，只让他计算一次
    // 同时在循环体里面判断退出或返回的条件，而不是在循环体外
    public int numSquares(int n) {
        Queue<Node> queue = new LinkedList<>();
        queue.add(new Node(n, 0));
        // 其实一个真正的图的 BSF 是一定会加上 visited 数组来过滤元素的
        boolean[] visited = new boolean[n+1];
        while (!queue.isEmpty()) {
            int num = queue.peek().val;
            int step = queue.peek().step;
            queue.remove();

            for (int i = 1; ; i++) {
                int a = num - i * i;
                if (a < 0) {
                    break;
                }
                // 若 a 已经计算到 0 了，就不必再往下执行了
                if (a == 0) {
                    return step + 1;
                }
                if (!visited[a]) {
                    queue.add(new Node(a, step + 1));
                    visited[a] = true;
                }
            }
        }
        return -1;
    }
```
## 515. 在每个树行中找最大值:BFS
在二叉树的每一行中找到最大的值。
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
## 994. 腐烂的橘子:BFS
```java
class Solution {
    int[] dr = new int[]{-1, 0, 1, 0};
    int[] dc = new int[]{0, -1, 0, 1};

    public int orangesRotting(int[][] grid) {
        int R = grid.length, C = grid[0].length;

        // queue : all starting cells with rotten oranges
        Queue<Integer> queue = new ArrayDeque();
        Map<Integer, Integer> depth = new HashMap();
        for (int r = 0; r < R; ++r)
            for (int c = 0; c < C; ++c)
                if (grid[r][c] == 2) {
                    int code = r * C + c;
                    queue.add(code);
                    depth.put(code, 0);
                }

        int ans = 0;
        while (!queue.isEmpty()) {
            int code = queue.remove();
            int r = code / C, c = code % C;
            for (int k = 0; k < 4; ++k) {
                int nr = r + dr[k];
                int nc = c + dc[k];
                if (0 <= nr && nr < R && 0 <= nc && nc < C && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    int ncode = nr * C + nc;
                    queue.add(ncode);
                    depth.put(ncode, depth.get(code) + 1);
                    ans = depth.get(ncode);
                }
            }
        }

        for (int[] row: grid)
            for (int v: row)
                if (v == 1)
                    return -1;
        return ans;

    }
}
```
## 646. 最长数对链
按end排序 然后每次选择能放的end最小的一个数对加入当前的数对链
```java
public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs,(a,b)-> a[1]-b[1]);
        int res = 1,tmp = pairs[0][1];
        for(int i = 1;i < pairs.length;i++){
            if(pairs[i][0] > tmp){
                 res++;
                 tmp = pairs[i][1];
            }
        }
        return res;
    }
```
##  547.省份数量：无向图最小连通数：并查集/DFS
```java
//无向图最小连通数

// //并查集
// class Solution {
//     public int findCircleNum(int[][] M) {
//         int s = M.length;
//         UnionFind uf = new UnionFind(s);
//         for(int i = 0;i<s-1;i++)
//             for(int j = i+1;j<s;j++)
//                 if(M[i][j] == 1) 
//                     uf.union(i,j);
//         return uf.getSetCount();
//     }
// }
    
//     public class UnionFind {
// 		private int[] id; // 表示当前下标是父亲是谁，如in[3] = 1, 3的父亲是1。
// 		private int[] size; 
// 		private int setCount; //连通个数
// 		private int maxSetSize; //最大的size
        
//         //初始化
// 		public UnionFind(int n) {
// 			id = new int[n];
// 			size = new int[n];
//             //父节点以及size的初始化
// 			for (int i = 0; i < n; i++) {
// 				id[i] = i; // self-loop
// 				size[i] = 1;
// 			}
// 			setCount = n;
// 			maxSetSize = 1;
// 		}

// 		// O(logN)
// 		public void union(int i, int j) {
// 			int ri = root(i);
// 			int rj = root(j);
// 			if (ri == rj)
// 				return;
//             //将个数小的合并到个数大的集合中
// 			if (size[ri] >= size[rj]) {
// 				id[rj] = id[ri];
// 				size[ri] += size[rj];
// 				maxSetSize = Math.max(maxSetSize, size[ri]);
// 			} else {
// 				id[ri] = id[rj];
// 				size[rj] += size[ri];
// 				maxSetSize = Math.max(maxSetSize, size[rj]);
// 			}
// 			setCount--;
// 		}

// 		// O(logN)
// 		public boolean find(int i, int j) {
// 			return root(i) == root(j);
// 		}

// 		public int getMaxSetSize() {
// 			return maxSetSize;
// 		}

// 		public int getSetCount() {
// 			return setCount;
// 		}

// 		private int root(int i) { // 不停向上寻找父节点
// 			while (id[i] != i) { // keep checking for the self-loop
// 				id[i] = id[id[i]]; // set grand-parent as parent (path compression)
// 				i = id[i]; // go up to parent
// 			}
// 			return i;
// 		}
// 	}


//DFS
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
## 323. 无向图中连通分量的数目:dfs
```java
class Solution {
    public int countComponents(int n, int[][] edges) {
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
## 934. 最短的桥:dfs/bfs
```java
class Solution {
    //dfs寻找第一个岛屿，bfs寻找最断桥
    int[][] dirs = {{-1,0},{1,0},{0,-1},{0,1}};
    Queue<int[]> queue;
    int[][] matrix;
    boolean[][] visited;
    int n;
    public int shortestBridge(int[][] A) {
        n = A.length;
        visited = new boolean[n][n];
        matrix = A; //让A变成一个全局变量
        queue = new LinkedList<>();
        boolean found = false; //控制dfs只寻找一个岛屿
        for (int i=0;i<n && !found;i++) {
            for (int j=0;j<n && !found;j++) {
                if (matrix[i][j] == 1) {
                    dfs(i,j);
                    found = true;
                }
            }
        }
        while (!queue.isEmpty()) {
            int[] cur = queue.poll(); //cur[1]表示当前步数
            int i = cur[0]/n;
            int j = cur[0]%n;
            for (int[] dir:dirs) {
                int x = i + dir[0];
                int y = j + dir[1];
                if (x < 0 || y < 0 || x >= n || y >= n || visited[x][y]) continue;
                if (matrix[x][y] == 1) return cur[1];
                visited[x][y]=true;
                queue.add(new int[]{x*n+y,cur[1]+1});
            }
        }
        return -1;
    }
    public  void dfs(int i, int j) {
        if (visited[i][j] || matrix[i][j] == 0) return;
        visited[i][j] = true;
        for (int[] dir:dirs) {
            int x = i+dir[0];
            int y = j+dir[1];
            if (x<0 || y<0||x>=n|| y>=n) continue;
            if (!visited[x][y] && matrix[x][y] == 0) {
                visited[x][y] = true;
                queue.add(new int[]{n*x+y,1});
            }
            dfs(x,y);
        }
    }
}
```
## 207. 课程表:有向图是否有环：拓扑排序/dfs
```java
class Solution {
    /*拓扑排序
    *每一次都输出入度为0的结点，并移除它、修改它指向的结点的入度
    *依次得到的结点序列就是拓扑排序的结点序列
    *如果图中还有结点没有被移除，则说明“不能完成所有课程的学习”。
    *拓扑排序保证了每个活动（在这题中是“课程”）的所有前驱活动都排在该活动的前面
    *并且可以完成所有活动。拓扑排序的结果不唯一。
    *拓扑排序还可以用于检测一个有向图是否有环
    *拓扑排序实际上应用的是贪心算法
    */

    // public boolean canFinish(int numCourses, int[][] prerequisites) {
    //     if (numCourses <= 0) 
    //         return false;
    //     int plen = prerequisites.length;
    //     if (plen == 0) 
    //         return true;
    //     int[] inDegree = new int[numCourses]; //保存入度
    //     for (int[] p : prerequisites) 
    //         inDegree[p[0]]++;
    //     LinkedList<Integer> queue = new LinkedList<>(); //保存入度为0的队列
    //     // 首先加入入度为 0 的结点
    //     for (int i = 0; i < numCourses; i++) {
    //         if (inDegree[i] == 0) {
    //             queue.addLast(i);
    //         }
    //     }
    //     // 拓扑排序的结果
    //     List<Integer> res = new ArrayList<>();
    //     while (!queue.isEmpty()) {
    //         Integer num = queue.removeFirst();
    //         res.add(num);
    //         // 把邻边全部遍历一下
    //         for (int[] p : prerequisites) {
    //             if (p[1] == num) {
    //                 inDegree[p[0]]--;
    //                 if (inDegree[p[0]] == 0) {
    //                     queue.addLast(p[0]);
    //                 }
    //             }
    //         }
    //     }
    //     return res.size() == numCourses;
    // }
    
    /*深度优先遍历
    *其实就是检测这个有向图中有没有环，只要存在环，这些课程就不能按要求学完
    */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (numCourses <= 0)
            return false;
        int plen = prerequisites.length;
        if (plen == 0)
            return true;
        int[] marked = new int[numCourses];

        // 初始化有向图
        HashSet<Integer>[] graph = new HashSet[numCourses];
        for (int i = 0; i < numCourses; i++) 
            graph[i] = new HashSet<>();

        // 有向图的 key 是前驱结点，value 是后继结点的集合
        for (int[] p : prerequisites) {
            graph[p[1]].add(p[0]);
        }

        for (int i = 0; i < numCourses; i++) {
            if (dfs(i, graph, marked)) {
                // 注意方法的语义，如果图中存在环，表示课程任务不能完成，应该返回 false
                return false;
            }
        }
        // 在遍历的过程中，一直 dfs 都没有遇到已经重复访问的结点，就表示有向图中没有环
        // 所有课程任务可以完成，应该返回 true
        return true;
    }

    //marked 如果 == 1 表示正在访问中，如果 == 2 表示已经访问完了
    //return true 表示图中存在环，false 表示访问过了，不用再访问了
    private boolean dfs(int i, HashSet<Integer>[] graph, int[] marked) {
        // 如果访问过了，就不用再访问了
        if (marked[i] == 1) {
            // 从正在访问中，到正在访问中，表示遇到了环
            return true;
        }

        if (marked[i] == 2) {
            // 表示在访问的过程中没有遇到环，这个节点访问过了
            return false;
        }
        // 走到这里，是因为初始化呢，此时 marked[i] == 0
        // 表示正在访问中
        marked[i] = 1;
        // 后继结点的集合
        HashSet<Integer> successorNodes = graph[i];

        for (Integer successor : successorNodes) {
            if (dfs(successor, graph, marked)) {
                // 层层递归返回 true ，表示图中存在环
                return true;
            }
        }
        // i 的所有后继结点都访问完了，都没有存在环，则这个结点就可以被标记为已经访问结束
        // 状态设置为 2
        marked[i] = 2;
        // false 表示图中不存在环
        return false;
    }
    
}
```
## 210. 课程表 II：返回有向图路径：拓扑排序/dfs
```java
class Solution {
    //拓扑排序
//     public int[] findOrder(int numCourses, int[][] prerequisites) {
//         if (numCourses <= 0)
//             return new int[0];
        
//         // 邻接表表示
//         HashSet<Integer>[] graph = new HashSet[numCourses];
//         for (int i = 0; i < numCourses; i++) {
//             graph[i] = new HashSet<>();
//         }
        
//         // 入度表
//         int[] inDegree = new int[numCourses];
        
//         // 遍历 prerequisites 的时候，把 邻接表 和 入度表 都填上
//         for (int[] p : prerequisites) {
//             graph[p[1]].add(p[0]);
//             inDegree[p[0]]++;
//         }
        
//         LinkedList<Integer> queue = new LinkedList<>();
//         for (int i = 0; i < numCourses; i++) {
//             if (inDegree[i] == 0) {
//                 queue.addLast(i);
//             }
//         }
        
//         ArrayList<Integer> res = new ArrayList<>();
//         while (!queue.isEmpty()) {
//             Integer inDegreeNode = queue.removeFirst();
//             res.add(inDegreeNode);
//             HashSet<Integer> nextCourses = graph[inDegreeNode];
//             for (Integer nextCourse : nextCourses) {
//                 inDegree[nextCourse]--;
//                 // 马上检测该结点的入度是否为 0，如果为 0，马上加入队列
//                 if (inDegree[nextCourse] == 0) {
//                     queue.addLast(nextCourse);
//                 }
//             }
//         }
        
//         // 如果结果集中的数量不等于结点的数量，就不能完成课程任务，这一点是拓扑排序的结论
//         int resLen = res.size();
//         if (resLen == numCourses) {
//             int[] ret = new int[numCourses];
//             for (int i = 0; i < numCourses; i++) {
//                 ret[i] = res.get(i);
//             }
//             //返回的是数组而不是ArrayList
//             return ret;
//         } else {
//             return new int[0];
//         }
//     }
    
    //DFS
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        if (numCourses <= 0) 
            return new int[0];
        int plen = prerequisites.length;
        if (plen == 0) {
            // 没有有向边，则表示不存在课程依赖，任务一定可以完成
            int[] ret = new int[numCourses];
            for (int i = 0; i < numCourses; i++) {
                ret[i] = i;
            }
            return ret;
        }
        int[] marked = new int[numCourses];
        
        // 初始化有向图 
        HashSet<Integer>[] graph = new HashSet[numCourses];
        for (int i = 0; i < numCourses; i++) 
            graph[i] = new HashSet<>();
        

        // 有向图的 key 是前驱结点，value 是后继结点的集合
        for (int[] p : prerequisites) {
            graph[p[1]].add(p[0]);
        }
        
        // 使用 Stack 或者 List 记录递归的顺序，这里使用 Stack
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < numCourses; i++) {
            if (dfs(i, graph, marked, stack)) {
                // 注意方法的语义，如果图中存在环，表示课程任务不能完成，应该返回空数组
                return new int[0];
            }
        }

        assert stack.size() == numCourses;
        int[] ret = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            ret[i] = stack.pop();
        }
        return ret;
    }

    private boolean dfs(int i,
                        HashSet<Integer>[] graph,
                        int[] marked,
                        Stack<Integer> stack) {

        if (marked[i] == 1) {
            // 从正在访问中，到正在访问中，表示遇到了环
            return true;
        }
        if (marked[i] == 2) {
            // 表示在访问的过程中没有遇到环，这个节点访问过了
            return false;
        }

        marked[i] = 1;

        HashSet<Integer> successorNodes = graph[i];
        for (Integer successor : successorNodes) {
            if (dfs(successor, graph, marked, stack)) {
                // 层层递归返回 true ，表示图中存在环
                return true;
            }
        }
        // i 的所有后继结点都访问完了，都没有存在环，则这个结点就可以被标记为已经访问结束
        // 状态设置为 2
        marked[i] = 2;
        stack.add(i);
        return false;
    }
    
    //拓扑排序
//     private int[] inDegree;
//     private boolean[][] graph;
//     private boolean[] onQueue;
//     private Queue<Integer> queue;
//     public int[] findOrder(int numCourses, int[][] prerequisites) {
//         inDegree=new int[numCourses];
//         graph=new boolean[numCourses][numCourses];
//         onQueue=new boolean[numCourses];
//         queue=new LinkedList<>();
//         //初始化
//         for(int i=0;i<prerequisites.length;i++){
//             graph[prerequisites[i][1]][prerequisites[i][0]]=true;
//             inDegree[prerequisites[i][0]]++;
//         }
//         int[] rt=new int[numCourses];
//         int count=0;
//         findNextInDegreeZero();
//         while(!queue.isEmpty()){
//             int v =queue.poll();
//             rt[count++]=v;
//             for(int i=0;i<graph[v].length;i++){
//                 if(graph[v][i]){
//                     inDegree[i]--;
//                 }
//             }
//             findNextInDegreeZero();
//         }
//         if(count==numCourses){
//          return rt;   
//         }
//         return new int[]{};
//     }
//     public void findNextInDegreeZero(){
//         for(int i=0;i<inDegree.length;i++){
//             if(inDegree[i]==0&&!onQueue[i]){
//                 onQueue[i]=true;
//                 queue.add(i);
//             }
//         }
//     }
}
```
## 5. 最长回文子串:中心扩展算法/DP/Manacher 算法(马拉车)
```java
class Solution {
    
    // 中心扩展算法
    // 2n-1个中心（偶数+奇数）
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1)
            return "";
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = expandAroundCenter(s, i, i); //奇数长度
            int len2 = expandAroundCenter(s, i, i + 1); //偶数长度
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    private int expandAroundCenter(String s, int left, int right) {
        int L = left, R = right;
        while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
            L--;
            R++;
        }
        return R - L - 1;
    }
    
    // //动态规划
    // public String longestPalindrome(String s) {
    //     int len = s.length();
    //     if(len<=1)
    //         return s;
    //     int longest = 1;
    //     String str = s.substring(0,1);
    //     boolean[][] dp = new boolean[len][len];
    //     for(int r=1;r<len;r++) {
    //         for(int l=0;l<r;l++) {
    //             if(s.charAt(l)==s.charAt(r) && (r-l<=2 || dp[l+1][r-1])) {
    //                 dp[l][r]=true;
    //                 if(r-l+1>longest) {
    //                     longest = r-l+1;
    //                     str = s.substring(l,r+1);
    //                 }
    //             }
    //         }
    //     }
    //     return str;
    // }
    
//     //Manacher 算法(马拉车)
//     //本质上还是中心扩散法
//     int len;
//     //插入字符，使长度始终未奇数，且不改变回文子串
//     private String generateSDivided(String s, char divide) {
//         if (s.indexOf(divide) != -1) {
//             throw new IllegalArgumentException("参数错误，您传递的分割字符，在输入字符串中存在！");
//         }
//         StringBuilder sBuilder = new StringBuilder();
//         sBuilder.append(divide);
//         for (int i = 0; i < len; i++) {
//             sBuilder.append(s.charAt(i));
//             sBuilder.append(divide);
//         }
//         return sBuilder.toString();
//     }

//     public String longestPalindrome(String s) {
//         len = s.length();
//         if (len == 0) {
//             return "";
//         }
//         String sDivided = generateSDivided(s, '#');
//         int slen = sDivided.length();
//         int[] p = new int[slen];
//         int mx = 0;
//         // id 是由 mx 决定的，所以不用初始化，只要声明就可以了
//         int id = 0;
//         int longestPalindrome = 1;
//         String longestPalindromeStr = s.substring(0, 1);
//         for (int i = 0; i < slen; i++) {
//             if (i < mx) {
//                 // 这一步是 Manacher 算法的关键所在，一定要结合图形来理解
//                 // 这一行代码是关键，可以把两种分类讨论的情况合并
//                 p[i] = Integer.min(p[2 * id - i], mx - i);
//             } else {
//                 // 走到这里，只可能是因为 i = mx
//                 if (i > mx) {
//                     throw new IllegalArgumentException("程序出错！");
//                 }
//                 p[i] = 1;
//             }
//             // 老老实实去匹配，看新的字符
//             //以i为中心，进行中心扩散
//             while (i - p[i] >= 0 && i + p[i] < slen && sDivided.charAt(i - p[i]) == sDivided.charAt(i + p[i])) {z
//                 p[i]++;
//             }
//             // 我们想象 mx 的定义，它是遍历过的 i 的 i + p[i] 的最大者
//             // 写到这里，我们发现，如果 mx 的值越大，
//             // 进入上面 i < mx 的判断的可能性就越大，这样就可以重复利用之前判断过的回文信息了
//             if (i + p[i] > mx) {
        //                 id = i;
//             }

//             if (p[i] - 1 > longestPalindrome) {
//                 longestPalindrome = p[i] - 1;
//                 longestPalindromeStr = sDivided.substring(i - p[i] + 1, i + p[i]).replace("#", "");
//             }
//         }
//         return longestPalindromeStr;
//     }
}
```
## 516. 最长回文子序列
```java
class Solution {
    public int longestPalindromeSubseq(String s) {
//         int n = s.length();
//         int[][] dp = new int[n][n];
        
//         //00, 11, 22, 33, 44
//         for(int i=0; i<n; i++){
//             dp[i][i] = 1;//从i到j的最长回文子序列长度
//         }
        
//         //01, 12, 23, 34
//         //02,13,24
//         //03,14
//         //04
//         for(int l=2; l<=n; l++){//l是子序列长度
//             for(int i=0; i<n-l+1; i++){//i是start
//                 int j = i+l-1;//j是end
//                 if(l==2 && s.charAt(j) == s.charAt(i)) dp[i][j] = 2;//长度为2
//                 else if (s.charAt(j) == s.charAt(i)) dp[i][j] = 2 + dp[i+1][j-1];
//                 else dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
//             }
//         }
//         return dp[0][n-1];//从0到n-1
        
        //改进 从后往前（应该和从前往后一样）
        char[] chars=s.toCharArray();
        int length=s.length();
        int[] current=new int[length];
        int[] pre =new int[length]; 
        for(int i=length-1;i>=0;i--){
            current[i]=1;
            for(int j=i+1;j<length;j++){
                if(chars[i]==chars[j]){
                    current[j]=pre[j-1]+2;
                }else{
                    current[j]=Math.max(current[j-1],pre[j]);
                }
            }
            int[] tmp=pre;
            pre=current;
            current=tmp;
        }
        return pre[length-1];
    }
}
```
## 10. 正则表达式匹配
### 题目要求
>- '.' 匹配任意单个字符
>- '*' 匹配零个或多个前面的那一个元素
### 代码
```java
class Solution {
//     //回溯
//     public boolean isMatch(String text, String pattern) {
//         if (pattern.isEmpty())
//             return text.isEmpty();
//         boolean first_match = (!text.isEmpty() &&
//                                (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.'));

//         if (pattern.length() >= 2 && pattern.charAt(1) == '*'){
//             return (isMatch(text, pattern.substring(2)) ||
//                     (first_match && isMatch(text.substring(1), pattern)));
//         } else {
//             return first_match && isMatch(text.substring(1), pattern.substring(1));
//         }
//     }
    
    //动态规划 自顶向下
    //其实只是想回溯的结果记录，即备忘录
    int[][] memo;//不能用boolean,无法判断是否已保存结果（==null）

    public boolean isMatch(String text, String pattern) {
        memo = new int[text.length() + 1][pattern.length() + 1];
        return dp(0, 0, text, pattern);
    }

    public boolean dp(int i, int j, String text, String pattern) {
        if (memo[i][j] != 0) {
            return memo[i][j]==1;
        }
        boolean ans;
        if (j == pattern.length()){
            ans = i == text.length();
        } else{
            boolean first_match = (i < text.length() &&
                                   (pattern.charAt(j) == text.charAt(i) ||
                                    pattern.charAt(j) == '.'));

            if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                ans = (dp(i, j+2, text, pattern) ||
                       first_match && dp(i+1, j, text, pattern));
            } else {
                ans = first_match && dp(i+1, j+1, text, pattern);
            }
        }
        memo[i][j] = ans?1:2;
        return ans;
    }

    
//     //动态规划 自底向上
//     public boolean isMatch(String text, String pattern) {
//         boolean[][] dp = new boolean[text.length() + 1][pattern.length() + 1];
//         dp[text.length()][pattern.length()] = true;

//         for (int i = text.length(); i >= 0; i--){
//             for (int j = pattern.length() - 1; j >= 0; j--){
//                 boolean first_match = (i < text.length() &&
//                                        (pattern.charAt(j) == text.charAt(i) ||
//                                         pattern.charAt(j) == '.'));
//                 if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
//                     dp[i][j] = dp[i][j+2] || first_match && dp[i+1][j];
//                 } else {
//                     dp[i][j] = first_match && dp[i+1][j+1];
//                 }
//             }
//         }
//         return dp[0][0];
//     }
}
```
## 44. 通配符匹配：双指针/动态规划
### 题目要求
>- '?' 可以匹配任何单个字符。
>- '*' 可以匹配任意字符串（包括空字符串）。
### 代码
```java
class Solution {
    //双指针
    // public boolean isMatch(String s, String p) {
    //     int sn = s.length();
    //     int pn = p.length();
    //     int i = 0;
    //     int j = 0;
    //     int start = -1;
    //     int match = 0;
    //     while (i < sn) {
    //         if (j < pn && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?')) {
    //             i++;
    //             j++;
    //         } else if (j < pn && p.charAt(j) == '*') {
    //             start = j;
    //             match = i;
    //             j++;
    //         } else if (start != -1) { //还没到*匹配结束的位置
    //             j = start + 1;
    //             match++;
    //             i = match;
    //         } else {
    //             return false;
    //         }
    //     }
    //     while (j < pn) { //i=sn 即i指针已结束，但j指针还没结束
    //         if (p.charAt(j) != '*') return false;
    //         j++;
    //     }
    //     return true;
    // }
    
    //动态规划
    public boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true;
        for (int j = 1; j < p.length() + 1; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 1];
            }
        }
        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 1; j < p.length() + 1; j++) {
                if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (p.charAt(j - 1) == '*') {
                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j]; //*为任意字符或空字符
                }
            }
        }
        return dp[s.length()][p.length()];
    }
}
```
## 202. 快乐数
### 题目要求
一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数。
### 代码
```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> bobo;
        while(!bobo.count(n)){
            int sum = 0;
            bobo.insert(n);
            while(n != 0){
                sum = sum + (n%10) * (n%10);
                n /= 10;
            }
            n = sum;
        }
        return n == 1;
    }
};

//递归
class Solution {
public:
    unordered_set<int> bobo;
    bool isHappy(int n) {
        int sum = 0;
        if(n == 1) return true;
        else if(bobo.count(n))  return false;
        else{
            bobo.insert(n);
            while(n != 0){
                sum = sum + (n%10) * (n%10);
                n /= 10;
            }
            n = sum;
        }
        return isHappy(n);
    }
};
```
## 261. 以图判树
### 题目要求
给定从 0 到 n-1 标号的 n 个结点，和一个无向边列表（每条边以结点对来表示），请编写一个函数用来判断这些边是否能够形成一个合法有效的树结构。
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

## 695. 岛屿的最大面积:最大的无向连通图/dfs
```java
class Solution {
    //最大的无向连通图 模板
    int max = 0;
    int r;
    int c;
    public int maxAreaOfIsland(int[][] grid) {
        r = grid.length;
        c = grid[0].length;
        if(r==0||c==0)
            return 0;
        for(int i=0;i<r;i++) {
            for(int j=0;j<c;j++) {
                if(grid[i][j] == 1) {
                    max = Math.max(max,dfs(i,j,grid));
                }
            }
        }
        return max;
    }
    
    //直接返回当前dfs的数目
    public int dfs(int i, int j,int[][] grid) {
        if(i<0 || i>=r || j<0 || j>=c || grid[i][j] == 0) //grid[i][j]==0一定要放在最后面，避免下标越界
            return 0;
        grid[i][j] = 0;
        return 1+dfs(i+1,j,grid)+dfs(i-1,j,grid)+dfs(i,j+1,grid)+dfs(i,j-1,grid);
    }
}
```
## 394. 字符串解码:栈
```java
class Solution {
    public String decodeString(String s) {
        StringBuilder res = new StringBuilder();
        Stack<Integer> numStack = new Stack<>();
        Stack<String> strStack = new Stack<>();
        String tempStr = null;
        for (int i = 0; i < s.length(); i++) { //i用来控制指针坐标
            char c = s.charAt(i);
            if (s.charAt(i) == ']') {
                String str = strStack.pop();
                int num = numStack.pop();
                String nowStr = repeatStr(str, num);
                if (!numStack.isEmpty()) {
                   StringBuilder builder = new StringBuilder();
                    builder.append(strStack.pop());
                   builder.append(nowStr);
                    //将后面不需要重复的字符（遇到下一个]和数字之前的字符）加到builder后面
                   int m = i + 1;
                   while (s.charAt(m) != ']' && !('0' < s.charAt(m) && '9' >= s.charAt(m))) {
                       m++;
                   }
                   builder.append(s.substring(i + 1, m));
                    strStack.push(builder.toString());
                   i = m - 1;
                } else {
                    tempStr = null;
                    res.append(nowStr);
                }
            } else if ('0' <= c && '9' >= c) {
                int m = i + 1;
                //可能是个多位数
                while ('0' <= s.charAt(m) && '9' >= s.charAt(m)) {
                    m++;
                }
                numStack.push(Integer.parseInt(s.substring(i, m)));
                i = m - 1;
                //得到下一个字符串
                int k =  i + 2;
                while (s.charAt(k) != ']' && !('0' <= s.charAt(k) && '9' >= s.charAt(k)))  {
                    k++;
                }
                strStack.push(s.substring(i+2, k));
                i = k - 1;
            } else if (numStack.isEmpty()) {
                res.append(s.charAt(i));
            }
        }
        return res.toString();
    }

    private String repeatStr(String str, int num) {
        StringBuilder sb = new StringBuilder();
        if (num <= 0) {
            return "";
        }
        for (int i = 0; i < num; i++) {
            sb.append(str);
        }
        return sb.toString();
    }
}
```
## 1. 两数之和
### 代码
```java
//两遍哈希
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
//一遍哈希
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
## 3. 无重复字符的最长子串：滑动窗口
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
//优化：使用 HashMap
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
//优化：假设字符集为 ASCII 128
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        int[] index = new int[128]; // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(index[s.charAt(j)], i);
            ans = Math.max(ans, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return ans;
    }
}
```
## 20. 有效的括号：栈
```java
class Solution {

  // Hash table that takes care of the mappings.
  private HashMap<Character, Character> mappings;

  // Initialize hash map with mappings. This simply makes the code easier to read.
  public Solution() {
    this.mappings = new HashMap<Character, Character>();
    this.mappings.put(')', '(');
    this.mappings.put('}', '{');
    this.mappings.put(']', '[');
  }

  public boolean isValid(String s) {

    // Initialize a stack to be used in the algorithm.
    Stack<Character> stack = new Stack<Character>();

    for (int i = 0; i < s.length(); i++) {
      char c = s.charAt(i);

      // If the current character is a closing bracket.
      if (this.mappings.containsKey(c)) {

        // Get the top element of the stack. If the stack is empty, set a dummy value of '#'
        char topElement = stack.empty() ? '#' : stack.pop();

        // If the mapping for this bracket doesn't match the stack's top element, return false.
        if (topElement != this.mappings.get(c)) {
          return false;
        }
      } else {
        // If it was an opening bracket, push to the stack.
        stack.push(c);
      }
    }

    // If the stack still contains elements, then it is an invalid expression.
    return stack.isEmpty();
  }
}
```
## 215. 数组中的第K个最大元素
```java
//大顶堆
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // init heap 'the smallest element first'
        PriorityQueue<Integer> heap =
            new PriorityQueue<Integer>((n1, n2) -> n1 - n2);

        // keep k largest elements in the heap
        for (int n: nums) {
          heap.add(n);
          if (heap.size() > k)
            heap.poll();
        }

        // output
        return heap.poll();        
  }
}
//快速选择（类似快排）
import java.util.Random;
class Solution {
  int [] nums;

  public void swap(int a, int b) {
    int tmp = this.nums[a];
    this.nums[a] = this.nums[b];
    this.nums[b] = tmp;
  }


  public int partition(int left, int right, int pivot_index) {
    int pivot = this.nums[pivot_index];
    // 1. move pivot to end
    swap(pivot_index, right);
    int store_index = left;

    // 2. move all smaller elements to the left
    for (int i = left; i <= right; i++) {
      if (this.nums[i] < pivot) {
        swap(store_index, i);
        store_index++;
      }
    }

    // 3. move pivot to its final place
    swap(store_index, right);

    return store_index;
  }

  public int quickselect(int left, int right, int k_smallest) {
    /*
    Returns the k-th smallest element of list within left..right.
    */

    if (left == right) // If the list contains only one element,
      return this.nums[left];  // return that element

    // select a random pivot_index
    Random random_num = new Random();
    int pivot_index = left + random_num.nextInt(right - left); 
    
    pivot_index = partition(left, right, pivot_index);

    // the pivot is on (N - k)th smallest position
    if (k_smallest == pivot_index)
      return this.nums[k_smallest];
    // go left side
    else if (k_smallest < pivot_index)
      return quickselect(left, pivot_index - 1, k_smallest);
    // go right side
    return quickselect(pivot_index + 1, right, k_smallest);
  }

  public int findKthLargest(int[] nums, int k) {
    this.nums = nums;
    int size = nums.length;
    // kth largest is (N - k)th smallest
    return quickselect(0, size - 1, size - k);
  }
}
```
## 703. 数据流中的第K大元素:最小堆
```java
class KthLargest {
    PriorityQueue<Integer> maxHeap; 
    int length;
    //维护一个大小为k的最小堆 堆顶即是第k大的元素
    public KthLargest(int k, int[] nums) {
        length = k;
        maxHeap = new PriorityQueue<Integer>(k);
        for(int num : nums){
            add(num);
        }
    }
    
    public int add(int val) {
        if(maxHeap.size() < length) {
            maxHeap.add(val);
            return maxHeap.peek();
        }
        else{
            if(val <= maxHeap.peek()){
                return maxHeap.peek();
            }
            else{
                maxHeap.poll();
                maxHeap.add(val);
                return maxHeap.peek();
            }
        }
    }
}
```
## 347. 前 K 个高频元素
```java
//最小堆
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        // 使用字典，统计每个元素出现的次数，元素为键，元素出现的次数为值
        HashMap<Integer,Integer> map = new HashMap();
        for(int num : nums){
            if (map.containsKey(num)) {
               map.put(num, map.get(num) + 1);
             } else {
                map.put(num, 1);
             }
        }
        // 遍历map，用最小堆保存频率最大的k个元素
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return map.get(a) - map.get(b);
            }
        });
        for (Integer key : map.keySet()) {
            if (pq.size() < k) {
                pq.add(key);
            } else if (map.get(key) > map.get(pq.peek())) {
                pq.remove();
                pq.add(key);
            }
        }
        // 取出最小堆中的元素
        List<Integer> res = new ArrayList<>();
        while (!pq.isEmpty()) {
            res.add(pq.remove());
        }
        return res;
    }
}
//桶排序法
//基于桶排序求解「前 K 个高频元素」
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        List<Integer> res = new ArrayList();
        // 使用字典，统计每个元素出现的次数，元素为键，元素出现的次数为值
        HashMap<Integer,Integer> map = new HashMap();
        for(int num : nums){
            if (map.containsKey(num)) {
               map.put(num, map.get(num) + 1);
             } else {
                map.put(num, 1);
             }
        }
        
        //桶排序
        //将频率作为数组下标，对于出现频率不同的数字集合，存入对应的数组下标
        List<Integer>[] list = new List[nums.length+1];
        for(int key : map.keySet()){
            // 获取出现的次数作为下标
            int i = map.get(key);
            if(list[i] == null){
               list[i] = new ArrayList();
            } 
            list[i].add(key);
        }
        
        // 倒序遍历数组获取出现顺序从大到小的排列
        for(int i = list.length - 1;i >= 0 && res.size() < k;i--){
            if(list[i] == null) continue;
            res.addAll(list[i]);
        }
        return res;
    }
}
//官方题解
class Solution {
  public List<Integer> topKFrequent(int[] nums, int k) {
    // build hash map : character and how often it appears
    HashMap<Integer, Integer> count = new HashMap();
    for (int n: nums) {
      count.put(n, count.getOrDefault(n, 0) + 1);
    }

    // init heap 'the less frequent element first'
    PriorityQueue<Integer> heap =
            new PriorityQueue<Integer>((n1, n2) -> count.get(n1) - count.get(n2));

    // keep k top frequent elements in the heap
    for (int n: count.keySet()) {
      heap.add(n);
      if (heap.size() > k)
        heap.poll();
    }

    // build output list
    List<Integer> top_k = new LinkedList();
    while (!heap.isEmpty())
      top_k.add(heap.poll());
    Collections.reverse(top_k);
    return top_k;
  }
}
```





