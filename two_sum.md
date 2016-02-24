# 1.Two Sum

[Two Sum](https://leetcode.com/problems/two-sum/)


>Given an array of integers, return indices of the two numbers such that they add up to a specific target.

>You may assume that each input would have exactly one solution.

>Example:

><pre>Given nums = [2, 7, 11, 15], target = 9,
>Because nums[0] + nums[1] = 2 + 7 = 9,
>return [0, 1].</pre>

##题目要求：
给定一个数列和一个target，找到数列中两个加和为target的数字，并输出其位置

##要求：
无

##解答：
我们可以用字典/哈希表/散列表，以数字的值为索引，位置为值，来存储每一个数字的位置。这样只要取到某个数字，并计算它与target的差值，并察看是否存在值为差值的元素，就可以找到对应的两个数字。

##代码：

``` python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        check = {}
        for i, num in enumerate(nums):
            if num not in check:
                check[target - num] = i
            else:
                return [min(i,check[num]), max(i,check[num])]
```

##分析：

时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">