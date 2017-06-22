# Burst Balloons

[burst Balloons](https://leetcode.com/problems/burst-balloons/)


>Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.
>
Find the maximum coins you can collect by bursting the balloons wisely.


####Example
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
一道比较
还是DP的问题。假设我们有数组DP[]\[],对于 DP[i]\[j],表示 i个人抄j本书所需的最少时间。可以得到 DP[1]\[j]的全部解。递推，可知 对于任意 DP[i]\[j]，有状态转移方程：

```
DP[i][j] = min(DP[i - 1][j],  // 完全不用第i个人抄写 
            max(DP[i - 1][j - 1], page[j]),  //让第i个人抄写第j份
            max(DP[i - 1][j - 2], page[j] + page[j - 1]),  //让第i个人抄写j, j - 1两份。
            ...,
            max(DP[i - 1][0], page[j] + page[j - 1] + ... + page[0]))//全部让第i个人写。
```
可解得最后 DP[k]\[pages.length]即为 k人抄所有书所需最短时间。

这里有一个可以优化的地方，因为对于 ```DP[1][j]``` 来说，它就是SUM(```pages[0 ... j]```)。所以我们计算在   ```pages[j] + pages[j - 1] + ... + pages[n]```   时，可以用 ```DP[1][j] - DP[1][n]```来代替。



##代码：

```java

public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: an integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
        // write your code here
        // DP[i][j] --> i person j pages, minimum time
        int [][] DP = new int[k + 1][pages.length + 1];
        DP[1][1] = pages[0];
        for (int i = 1; i < pages.length + 1; ++i){
            DP[1][i] = DP[1][i - 1] + pages[i - 1];
        }
        // BTW, DP[1][j] = SUM[j - 1](pages)
        
        for (int i = 2; i < k + 1; ++i){
            for (int j = pages.length; j >= 0; -- j){
                //suppose this guy gain nothing;
                DP[i][j] = DP[i - 1][j];
                //then from [l] to [l, 0], try to match min
                for (int l = j; l > 0; --l){
                    DP[i][j] = Math.min(DP[i][j], 
                                  Math.max(DP[i - 1][l] , DP[1][j] - DP[1][l]) );
                }
            }
        }
        return DP[k][pages.length];
    }
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n^3)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(n^2)" style="border:none;">
