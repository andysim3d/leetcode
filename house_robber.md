# 198. House Robber

[House Robber](https://leetcode.com/problems/house-robber/)

>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

##题目要求：

>你是一个窃贼，打算偷窃一整条街的居民。每个房子都有一定的钱。你不能偷窃相邻的两个房子，因为这样会引发警报。给你所有房子的钱，计算出你在不引发警报的情况下最多能偷窃多少

##特殊要求：
无

##解法：
经典的动态规划题目。

可见，对于任意房子，它的状态只有*被偷*与*不被偷*两种。

即，对于第i家住户，我们的收获应为：不偷它（即与i-1家偷到的一样，F(i-1)），或偷它（即不能偷i-1家，F(i-2) + W(i)）中较大的一个。

那么，我们可以推出其状态转移方程为


$$F(i) = max(F(i-1), F(i - 2) + W(i))$$

状态转移方程有了，代码也就出来了。

##代码

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        if len(nums) <= 2:
            return max(nums)
        l0 = nums[0]
        l1 = max(nums[0], nums[1])
        for i in range(2, len(nums)):
            l2 = max(l0 + nums[i], l1)
            l0, l1 = l1, l2
            
        return max(l0, l1)

```

##分析：

时间复杂度： $$O(n)$$

空间复杂度： $$O(1)$$