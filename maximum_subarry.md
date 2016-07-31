# 53.Maximum Subarry
[Maximum Subarry](https://leetcode.com/problems/maximum-subarray/)


>Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

>For example, given the array ```[−2,1,−3,4,−1,2,1,−5,4],```
the contiguous subarray ```[4,−1,2,1]``` has the largest sum = ```6```.


##题目要求：
给定数组，计算其最大子序列

##要求：
无

##解答：
动态规划类题目。动态转移方程为：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large F(i) = Max(num(i), F(i- 1) %2Bnum(i))" style = "border:none;">


##代码：

python:
```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        Max = 0
        RealMax = nums[0]
        for i, num in enumerate(nums):
            Max = max(num, Max + num)
            RealMax = max(RealMax, Max)

        return RealMax
```

Java:
```Java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(int[] nums) {
        // write your code
        if (nums == null || nums.length < 1){
            return 0;
        }
        if (nums.length == 1){
            return nums[0];
        }
        int max = nums[0];
        int curr = 0;
        for ( int i : nums){
            if (curr + i < i){
                curr = i;
            }
            else{
                curr += i;
            }
            max = Math.max(curr, max);
        }
        return max;
    }
}
```
##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">

#Maximum Subarray II
[Maximum Subarray II](http://www.lintcode.com/en/problem/maximum-subarray-ii/)
>Given an array of integers, find two non-overlapping subarrays which have the largest sum.
>
The number in each subarray should be contiguous.
>
Return the largest sum.

##题目要求：
和 I 的要求一样，只是这次要求的是2个子集的和。

##要求：
无

##解答：
这道题的解法是，建立两个数组 L2R 和 R2L，L2R[i] 表示从0 到 i的最大连续子集和。R2L表示从i到最后一个元素的最大连续子集和。

那我们只需要遍历 1 到 倒数第二个元素， 并计算 L2R[i] + R2L[i + 1]的值，记录最大值，就是2个子集的最大和。

##代码：

```Java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: An integer denotes the sum of max two non-overlapping subarrays
     */
    public int maxTwoSubArrays(ArrayList<Integer> nums) {
        // write your code
        if (nums == null || nums.size() < 1){
            return 0;
        }
        int length = nums.size() ;
        //leftToRight[i] means max value from 0 to i
        int [] leftToRight = new int[length + 1];
        //rightToleft means max value from i + 1 to length;
        int [] rightToLeft = new int[length + 1];
        int max = Integer.MIN_VALUE;
        int tmp = 0;
        for (int i = 0; i < length - 1; ++i){
            tmp += nums.get(i);
            if (tmp < nums.get(i)){
                tmp = nums.get(i);
            }
            max = Math.max(max, tmp);
            leftToRight[i] = max;
        }
        max = Integer.MIN_VALUE;
        tmp = 0;
        for (int i = length - 1; i >= 0; --i){
            tmp += nums.get(i);
            if (tmp < nums.get(i)){
                tmp = nums.get(i);
            }
            max = Math.max(max, tmp);
            rightToLeft[i] = max;
        }
        max = Integer.MIN_VALUE;
        for (int i = 0; i < length  - 1; ++i){
            max = Math.max(max, leftToRight[i] + rightToLeft[i + 1]);
        }
        return max;
    }
}
```
##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">


#Maximum Subarray III
[Maximum Subarray III](http://www.lintcode.com/en/problem/maximum-subarray-iii/)

>Given an array of integers and a number k, find k non-overlapping subarrays which have the largest sum.
>
The number in each subarray should be contiguous.
>
Return the largest sum.

####题目要求：
和 I, II 的要求一样，只是这次要求的是N个子集的和。

##要求：
无

##解答：
对于这道题，我们的解法依然是DP：

我们需要建立两个二维数组，分别是 DP[]\[] 和 local []\[]。 其中， local[i]\[j]表示前i个数里分成j个集合且一定会取到i的最大子集和。DP[i]\[j]表示前i个数里分成j个集合，可以不取到i的最大子集和。

所以状态转移方程为：

```
  local[i][j] = max(local[i - 1][j], DP[i - 1][j - 1](确保DP一定没有取到i)) + nums[i - 1];
  DP[i][j] =  if i == j: local[i][j] (确保必须没有空集)。
              else: max(local[i][j], DP[i - 1][j]);
```

##代码：

```Java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: An integer denote to find k non-overlapping subarrays
     * @return: An integer denote the sum of max k non-overlapping subarrays
     */
    public int maxSubArray(int[] nums, int k) {
        // write your code here
        int length = nums.length;
        //DP [i][j] means from [0] -> [i], pick [j] sets' maxvalue
        int [][] DP = new int[k + 1][length + 1];
        //local [i][j] means from [0] -> [i], pick [j] set, including i, maxvalue;
        int [][] local = new int[k + 1][length + 1];
        // states transfer function:
        // local[i][j] = max(local[i - 1][j] ,DP[i - 1][j - 1] ) + nums[i]
        // DP[i][j] = max(DP[i -1][j], local[i][j])
        for (int i = 1; i < k + 1; ++i){
            local[i][i - 1] = Integer.MIN_VALUE;
            for (int j = i; j < length + 1; ++j){
                local[i][j] = Math.max(local[i][j - 1], DP[i - 1][j - 1]) + nums[j - 1];
                if (j == i)
                    DP[i][j] = local[i][j];
                else
                    DP[i][j] = Math.max(DP[i][j-1], local[i][j]);
            }
        }
        return DP[k][nums.length];
    }
}
```

##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n^2) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n^2) " style="border:none;">
