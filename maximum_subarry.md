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

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large F(i) = Max(num, F(i- 1) \+num(i)" style = "bold">



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