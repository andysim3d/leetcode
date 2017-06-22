# Burst Balloons

[burst Balloons](https://leetcode.com/problems/burst-balloons/)


>Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.
>
Find the maximum coins you can collect by bursting the balloons wisely.



###Example
>Given [3, 1, 5, 8]
>
Return 167

    nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
    coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167


##题目要求：
给出一个数组代表一排气球，每个气球有自己的分数。
得分的计算方法为，**被击破的气球的分数\*被击破气球左边的气球的分数\*被击破气球右边的分数** 
若一个气球没有左/右时，假设其左/右为1.
求击破所有气球后最多可以得到的分数。

##要求：
无
##解法：
一道比较不好想的DP题目。采取从顶至下的方法，我们假设只剩最后一个气球，那么它的分数就是**它本身 \* 1 \*1 = 它本身的分数**。
接下来，我们可以将数组分成 最后一个气球左边，和最后一个气球右边两部分。对于左边，我们可知其右边界是最后一个气球（分治法分析）。同理，其右侧的左边界也是最后一个气球。
那么，我们可以假设，DP[i]\[j]代表，i为左边界，j为右边界所能得到的最高分数。
则， 对于DP[i]\[j],有着：

```python
DP[i][j] = max(DP[i][j],
            max([num[i] * num[k] * num[j] + DP[i][k] + DP[k][j] for k in range(i+1, j)])
)

```

即， 对于任意位置的k, 我们都可以计算num[k]\*num[i]\*num[j] + DP[i]\[k] + DP[j]\[k]的值（假设 k为最后打破的气球）。并从 i+1 到 j，寻找能使其和最大的k。

可优化的点： 因为分数计算是乘法，则我们可以在预处理时去掉所有的0 来减少元素大小。


##代码：

```python
    class Solution(object):
        def maxCoins(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            nums = [1] + [i for i in nums if i > 0] + [1]
            n = len(nums)
            dp = [[0] * n for _ in xrange(n)]
            for k in xrange(2, n):
                for left in xrange(0, n - k):
                    right = left + k
                    dp[left][right] = \
                        max([nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right] for i in xrange(left+1, right)])
                    
            return dp[0][n - 1]

```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n^3)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(n^2)" style="border:none;">
