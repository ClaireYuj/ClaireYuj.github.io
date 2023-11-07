---
title: Leetcode101-动态规划
top: false
cover: false
author: DULULU oO
date: 2023-03-20 14:07:01
password:
summary: 
tags: leetcode
categories: Leetcode101
---

动态规划（Dynamic Programming，简称 DP）是一种优化技巧，用于解决具有重叠子问题和最优子结构的问题。
它**将问题分解为更小的子问题**，将子问题的解存储在表中，然后**使用这些子问题的解**来构建**原问题的解**。

在动态规划中，有两种常见的方法：**自顶向下**（Top-down，也称为记忆化搜索）和**自底向上**（Bottom-up，也称为递推式方法）。

在斐波那契数列中的动态规划如下：

F(0) = 0
F(1) = 1
F(n) = F(n-1) + F(n-2)，当 n > 1 时

自顶而下

```java
import java.util.HashMap;
import java.util.Map;

public class FibonacciTopDown {
    public static void main(String[] args) {
        int n = 10;
        Map<Integer, Integer> memo = new HashMap<>();
        System.out.println(fibTopDown(n, memo));
    }

    public static int fibTopDown(int n, Map<Integer, Integer> memo) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        } else if (!memo.containsKey(n)) {
            memo.put(n, fibTopDown(n - 1, memo) + fibTopDown(n - 2, memo));
        }
        return memo.get(n);
    }
}

```

自底而上

```java
public class FibonacciBottomUp {
    public static void main(String[] args) {
        int n = 10;
        System.out.println(fibBottomUp(n));
    }

    public static int fibBottomUp(int n) {
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        }

        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
}

```

## 一维动态规划

### [70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢

>   输入：n = 2
    输出：2
    解释：有两种方法可以爬到楼顶。
    1. 1 阶 + 1 阶
    2. 2 阶


#### 动态规划

dp最简单的方式，可以通过计算已知量，求解未知量

```java
    public static int climbStairs(int n) {
        int[] dp =  new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i<=n; i++){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
```

#### 可以用递归，但会超时

```java
    public static int climbStairsReverse(int n) {
        if (n <= 1) return 1;
        return climbStairsReverse(n-1)+climbStairsReverse(n-2);
    }
```

### [198.打家劫舍](https://leetcode.cn/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

> 示例 1：
    输入：[1,2,3,1]
    输出：4
    解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。


该题的dp函数是 **dp[n] = max(dp[n-1], dp[n-2]+num[n])**

```java
class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        // 对dp的初始化要注意上下界
        if (len==0) return 0;
        if(len == 1) return nums[0];
        int[] dp = new int[len];
        int max_index = 0;
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for(max_index=2; max_index < len ; max_index++){
            
            if(dp[max_index-1] > dp[max_index-2]+nums[max_index]){
                dp[max_index] = dp[max_index-1]; 
            }
            else{
                dp[max_index] = nums[max_index]+dp[max_index-2];
            }
        }
        // if (dp[len-1]==0) dp[len-1] = Math.max(dp[max_index-2] + nums[len-1], dp[max_index-1]);
        return dp[len-1];
    }
}
```

虽然这样解答成功，但代码略有冗余，可以进行优化如下（直接遍历，而不需要使用下标）：

```java

public int rob(int[] nums) {
    int prev = 0;
    int curr = 0;

    // 每次循环，计算“偷到当前房子为止的最大金额”
    for (int i : nums) {
        // 循环开始时，curr 表示 dp[k-1]，prev 表示 dp[k-2]
        int temp = Math.max(curr, prev + i);
        prev = curr;
        curr = temp;
        // 循环结束时，curr 表示 dp[k]，prev 表示 dp[k-1]
    }

    return curr;
}

```

### [413.等差数列划分](https://leetcode.cn/problems/arithmetic-slices/)

如果一个数列 至少有三个元素 ，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，[1,3,5,7,9]、[7,7,7,7] 和 [3,-1,-5,-9] 都是等差数列。
给你一个整数数组 nums ，返回数组 nums 中所有为等差数组的子数组个数。

子数组是数组中的一个连续序列。

>   示例：
    输入：nums = [1,2,3,4]
    输出：3
    解释：nums 中有三个子等差数组：[1, 2, 3]、[2, 3, 4] 和 [1,2,3,4] 自身。


```java

class Solution {
  public static int numberOfArithmeticSlices(int[] nums) {
        int len = nums.length;
        if (len < 3) return 0;
        int sum_combin = 0;
        int count = 0;
        int cur_diff = nums[1]-nums[0];
        int pre_diff = cur_diff;
        for(int i = 2; i < len; i++){
            cur_diff = nums[i] - nums[i-1];
            if(cur_diff == pre_diff){
                count++;
            }
            else{
                sum_combin += getSum(Math.max(0, count));
                count = 0;
                pre_diff = cur_diff;

            }

        }
        sum_combin += getSum(Math.max(0, count));
        return sum_combin;
    }

    public static int getSum(int count){
        return (1+count)*count/2;
    }
    
}
```

## 二维动态规划

### [60.最短路径和](https://leetcode.cn/problems/minimum-path-sum/)

给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

![示例](/img/posts/Leetcode/leetcode64.jpg)

> 示例
    输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
    输出：7
    解释：因为路径 1→3→1→1→1 的总和最小 


类Dijkstra，由于要求最短路径，只能接受从上或从左路径输入

**dp[a][b] = min(dp[a-1][b], dp[a][b-1]) + num[a][b]**

```java

class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int dp[][] = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i = 1; i < m; i++){
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        for(int i = 1; i < n; i++){
            dp[0][i] = dp[0][i-1] + grid[0][i];
        }
        for(int i = 1;i < m; i++){
            for(int j = 1;j < n; j++){
                dp[i][j] = Math.min(dp[i-1][j]+grid[i][j], dp[i][j-1]+grid[i][j]);
            }
        }

        return dp[m-1][n-1];
    }
}
```

### [542.01矩阵](https://leetcode.cn/problems/01-matrix/)

给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 

![示例](/img/posts/Leetcode/leetcode542.jpg)

#### 思路

![dp公式](/img/posts/Leetcode/leetcode542_formula.jpg)

第一步，先把dp[][]数组填满，原矩阵为0的地方，dp矩阵也为0，其余的为10001
第二步，从左上角开始迭代，对比
1. 自身和**右侧元素+1**
2. 自身和**下方元素+1**

![左上角开始迭代](/img/posts/Leetcode/leetcode542_step1.jpg)

第三步，从右下角开始迭代，对比
1. 自身和**左侧元素+1**
2. 自身和**上方元素+1**

![右下角开始迭代](/img/posts/Leetcode/leetcode542_step2.jpg)

#### 代码

```java

public class Solution542 {
    public static int[][] updateMatrix(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int dp[][] = new int[m][n];
        // initiate
        for(int i = 0; i < m; i++){
            for(int j = 0;j < n; j++){
                dp[i][j] = mat[i][j] == 0? 0: 10001;
            }
        }

        // From left-top, compare left and top with self
        for(int i = 0; i < m ; i++){
            for(int j = 0; j < n ; j++){
                if(i > 0)
                   {
                        dp[i][j] = Math.min(dp[i][j], dp[i-1][j]+1);
                        
                   }
                if(j > 0){
                    dp[i][j] = Math.min(dp[i][j], dp[i][j-1]+1);
                   
                }
                   

            }
        }

        //From right-bottom, compare right and bottom with self
        // be careful about index, it is m-1 and n-1
        for(int i = m - 1; i >= 0; i--){
            for(int j = n - 1; j >= 0; j--){
                if(i + 1 < m){
                    dp[i][j] = Math.min(dp[i][j], dp[i+1][j]+1);
                }  
                if(j + 1 < n){
                    dp[i][j] = Math.min(dp[i][j], dp[i][j+1]+1);
                }
                    
            }
        }

        return dp;
    }
```

### [221.最大正方形](https://leetcode.cn/problems/maximal-square/)


在一个由 '0' 和 '1' 组成的二维矩阵内，找到只包含 '1' 的最大正方形，并返回其面积。

![示例](/img/posts/Leetcode/leetcode221.jpg)

可以观察到，当一个元素是1时，他的dp数组的值取决于**左侧，左上，上方dp数组值的最小值** 
> 当matrix[i][j] = 1时，dp[i][j] = 该元素min（左上，左侧，上方）+ 1

我们可以先初始化dp数组的第一行与第一列，然后按照dp函数迭代
![](/img/posts/Leetcode/leetcode221_step1.jpg)


```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        if (matrix == null || m == 0 || n == 0) {
            return 0;
        }

        int[][] dp = new int[m][n];
        if(matrix[0][0] == '0') dp[0][0] = 0;
        else dp[0][0] = 1;
        int max = dp[0][0];
        //初始化第一列
        for(int i = 0; i < m; i++){
            
            dp[i][0] = returnNum(matrix[i][0]);
            if(dp[i][0] > max) max = dp[i][0];
        }

        //初始化第一行
        for(int j = 0; j < n; j++){
            dp[0][j] = returnNum(matrix[0][j]);
            if(dp[0][j] > max) max = dp[0][j];
        }

        //当matrix[i][j] = 1时，dp[i][j] = 该元素min（左上，左侧，上方）+ 1
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                if(returnNum(matrix[i][j])==1)
                    dp[i][j] = getMin(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1; // dp表达式
                else dp[i][j] = 0;
                if(dp[i][j] > max) max = dp[i][j];
            }
        }
        return max*max;
    }

    public static int getMin(int a, int b, int c){
        int min_ab = Math.min(a, b);
        int min_ac = Math.min(a, c);
        return Math.min(min_ac, min_ab);
    }

    public static int returnNum(char c) {
        return c =='0'?0:1;
        
    }
    
}
```

## 切割问题

### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

> 示例
    输入：n = 12
    输出：3 
    解释：12 = 4 + 4 + 4

> 示例
    输入：n = 13
    输出：2
    解释：13 = 4 + 9

转移方程: **f(n) = 1 + min(f(n - j*j))** (j小于n开方)

仍然是用**数组来存每个状态**

```java
    public static int numSquares(int n) {
        int f[] = new int[n+1];
        if(n == 0) return 0;
        f[0] = 0; // 状态初态
        for(int i = 1; i <= n;i++){
            int min = 10001;
            for(int j = 1; j <= Math.sqrt(i); j++){
                min = min > 1 + f[i - j*j]?1 + f[i - j*j]:min; // 转移方程
            }
            f[i] = min;
        }
        return f[n];
    }


```


### [91.解码方法](https://leetcode.cn/problems/decode-ways/)

一条包含字母 A-Z 的消息通过以下映射进行了 编码 ：

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"11106" 可以映射为：

"AAJF" ，将消息分组为 (1 1 10 6)
"KJF" ，将消息分组为 (11 10 6)
注意，消息不能分组为  (1 11 06) ，因为 "06" 不能映射为 "F" ，这是由于 "6" 和 "06" 在映射中并不等价。

给你一个只含数字的 非空 字符串 s ，请计算并返回 解码 方法的 总数 。

题目数据保证答案肯定是一个 32 位 的整数。

> 示例
    输入：s = "12"
    输出：2
    解释：它可以解码为 "AB"（1 2）或者 "L"（12）。


分两种情况
> 第一种情况是我们使用了一个字符，**f[n] += f[n-1]**

> 第二种情况使用两个字符, **f[n] += f[n-2]**

![以三个字符为例](/img/posts/Leetcode/leetcode91_step1.jpg)

```java

class Solution {
    public static int numDecodings(String s) {
        if(s.charAt(0)== '0') return 0;
        int count = 1;
        int dp[] = new int[s.length()+1];
        dp[0] = 1;
        for(int i = 1; i <= s.length();++i){
            if (s.charAt(i - 1) != '0') {
                dp[i] += dp[i - 1];
            }
            if (i > 1 && isVaild(s.substring(i-2,i))==1) {
                dp[i] += dp[i - 2];
            }
            
        }
        return dp[s.length()];
    }

    public static int isVaild(String str){
        int num = Integer.parseInt(str);
        return num < 27 && num > 9? 1:0;
    }
}
```


### [139.单词拆分](https://leetcode.cn/problems/word-break/)

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出 s 。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

> 输入: s = "applepenapple", wordDict = ["apple", "pen"]
    输出: true
    解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。注意，你可以重复使用字典中的单词。


#### 思路

此时用到异或之前的状态
**dp[i]=dp[i] ∣∣ dp[i−word.length()]**
比如 s = applepenapple, 当识别到 str[i-word.length, i]可以构成一个wordDict中的单词时，如果dp[i] = true，需要满足：
dp[i-word.length] = true

但不能粗暴的写成dp[i] = dp[i-strLen]==true?true:false;
因为如果wordDict时[apple,pen,le]，当识别到apple中的le时，会进行dp[i-word.length]的判断，但此时l并不满足条件，所以dp[i]会变成false，显然不对

我们需要找到一个只要dp[i]=true了，就不会再改变的方式
所以采用**异或**，**dp[i] |= dp[i-strLen]**

#### 代码
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int len = s.length();
        boolean[] dp = new boolean[len+1];
        dp[0] = true;
        for(int i = 1;i<=len;i++){
            for(String str: wordDict){
                int strLen = str.length();
                if(i>=strLen){
                    String substr = s.substring(i-strLen,i);
                    if(substr.equals(str))  
                        // dp[i] = dp[i-strLen]==true?true:false;
                          dp[i] |= dp[i-strLen];
                }
                // dp[i] = false;
            }
        }
        return dp[len];
    }
}
```

## 回文

回文问题可以用dp来判定，假设boolean dp[1][3]记录了从1->3的子字符串是否为回文串，那么判断dp[0][4]是否为回文串可以根据
> dp[0][4] = (s.charAt(0)==s.charAt(4) && dp[1][3])?true:false;

整理为通用公式**dp[start][end] = (s[start]==s[end] && dp[start+1][end-1])**

### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

给你一个字符串 s，找到 s 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

示例 1：
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

示例 2：

输入：s = "cbbd"
输出："bb"

#### 代码

```java
class Solution {
    public String longestPalindrome(String s) {
        boolean dp[][] = new boolean[s.length()][s.length()];
        int max = 1, start = 0, end = 0;;
        for(int right = 1;right<s.length();right++){
            for(int left = 0; left < s.length();left++){
                if(s.charAt(left) == s.charAt(right) && (right - left <=2 || dp[left+1][right-1])){
                    dp[left][right] = true;
                    if(right-left+1 > max){
                        max = right-left;
                        start = left;
                        end = right;
                    }
                }
            }
        }
        return s.substring(start, end+1);
    }
}

```

#### 注意事项

该题中判断回文的起始状态要分三种情况 **dabac**, **dabbc**, **dbbac**,也就是中心扩散，中心右扩散和中心左扩散
所以在判断条件处需要或一个(right-left<=2),也就是任意满足一种扩散条件都可以

java类中substring的用法：s.substring(0,2)只包含s[0]和s[1]，所以如果要返回s[0-2],需要写作s.substring(0,3)


## [42.接雨水](https://leetcode.cn/problems/trapping-rain-water/)

![](img/posts/Leetcode/leetcode42.jpg)
### 贪心

先左遍历，当最后一个子容器为[高，低]而非[高，低，高]时，也就是最后一个子容器没有封口时，再右遍历

```java
class Solution {
    public int trap(int[] height) {
        int max = height[0];
        int sum = 0, subsum = 0;
        // 先从左往右，把整个容器分为多个[高，低，高]容器，获得所有形如[高，低，高]的容器中的容量
        // 但此时需要注意几种特殊情况
        // 1. 结尾形如[2,4,3,2],此时最后三个并没有形成[高，低，高]，而是[高，低]，因此最后一个容器并没有封口
        for(int i = 1;i<height.length;i++){
            if(height[i] < max){
                subsum += max - height[i];
            }
            else{
                max = height[i];
                sum += subsum;
                subsum = 0;
            }
        }
        
        // 当最后一个容器没有封口时，也就是subsum!=0，此时有形如[高，低]的容器在尾部
        // 从右往左遍历，直到遇到[高，低]容器中的最高点
        // 注意边界， while的边界要 < 最高点
        // 可能导致最后的submax仍然不为0
        if(subsum != 0){
            subsum = 0;
            int right_max = height[height.length-1];
            int i = height.length-2;
            while(height[i] < max){
                if(height[i] < right_max){
                    subsum += right_max - height[i];
                }
                else{
                    right_max = height[i];
                    sum += subsum;
                    subsum = 0;
                }
                i--;
            }
        }
        // 当形如[4,2,3]时，由于从左往右遍历时，[高，低]没有封口
        // 从右往左遍历时，最后的subsum不为0
        // 所以在最后的返回值处要在sum的基础上加上subsum
        return sum+subsum;
    }
}
```

### dp

下标为i处的高度可以接到的最多的雨水等于**其左边最大值与其右边最大值两者取小**减去i处的高度，因为下标为i的柱子处在左边最大值与右边最大值围成的区域中，这个区域可以接到的雨水高度只取决于左边最大值的柱子，那么我们再用左边最大值的柱子减去i处的高度就得到了下标为i处的高度可以接到的最多的雨水

使用两个数组，leftMax[i], rightMax[i]分别表示包括柱子i的左边最大值，与右边最大值

初始状态为：
> leftMax[0] = height[0];
  rightMax[height.length - 1] = height[height.length - 1];

递推公式为：

> leftMax[i] = max(leftMax[i - 1], height[i]);
  rightMax[i] = max(rightMax[i + 1], height[i]);

```java
class Solution {
    public int trap(int[] height) {
         int left_max[] = new int[height.length];
        int right_max[] = new int[height.length];

        left_max[0] = height[0];
        right_max[height.length-1] = height[height.length-1];

        for(int i = 1; i <height.length;i++) left_max[i] = Math.max(left_max[i-1], height[i]);
        for(int j = height.length-2; j >= 0;j--) right_max[j] = Math.max(right_max[j+1], height[j]);

        int maxSize = 0;
        for(int i = 0; i < height.length; i ++)
        {
            //如果左边最大值或者右边最大值小于等于i处的柱子的高度，那么显然这个柱子本身作为了区域的边界
            //那么它可以接到的雨水显然为0
            if(left_max[i] <= height[i] || right_max[i] <= height[i]) continue;
            else maxSize += Math.min(left_max[i], right_max[i]) - height[i];
        }

        return maxSize;

    }
}
```