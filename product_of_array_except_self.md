 238.Product of Array Except Self

[ Product of Array Except Self ](https://leetcode.com/problems/product-of-array-except-self/)

>Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

>Solve it **without division** and in O(n).

>For example, given [1,2,3,4], return [24,12,8,6].

>**Follow up**:

>Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)


##题目要求：
给定一个数列，返回一个数列，数列中每个值为除当前元素外的所有元素乘积。

##要求：
1. 不能使用除法
2. 不使用额外空间

##解法：
先不考虑两种特殊要求的情况下，我们需要考虑数列中有几个0。

没有0：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large F(i) = \frac{ \prod_{ i = 0 }^{ n } Num(i)}{Num(current)}" style="border:none;">

有一个0：

**除0项为所有非0元素乘积，其他所有元素均为0**

有两个0：

**全部为0**

那么，简单解的代码如下：
##代码1：
```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if nums == []:
            return []
        if nums.count(0) >= 2:
            return [0 for i in nums]
        if nums.count(0) == 1:
            p = nums.index(0)
            sumary = 1
            for i in nums:
                if i == 0:
                    continue
                sumary *= i
            q = [0 for i in nums]
            q[p] = sumary
            return q
        sumary = 1
        for i in nums:
            sumary *= i
        return [sumary / i for i in nums]
```

