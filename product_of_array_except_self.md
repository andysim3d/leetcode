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
2. 不使用额外空间（重新分配的返回数组不算在内）

##解法1：
先不考虑两种特殊要求的情况下，我们需要考虑数列中有几个0。

没有0：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large F(current) = \frac{ \prod_{ i = 0 }^{ n } Num(i)}{Num(current)}" style="border:none;">

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
##解法2：
考虑到第一个限制条件，即不能使用除法。可以想到，对于每个元素，计算它的乘积即可（会超时）。那么合理的想法就是优化乘法。

假设我们构建一个队列，对于其中任意一个元素，满足
<img src="http://chart.googleapis.com/chart?cht=tx&chl=\large InOrder(current) =  \prod_{ i = 0 }^{ current } Num(i)}" style="border:none;">,

如果我们再构建一个队列，满足<img src="http://chart.googleapis.com/chart?cht=tx&chl=\large ReOrder(current) =  \prod_{ i = current }^{ n } Num(i)}" style="border:none;">

那么，我们要的结果即为
<img src="https://latex.codecogs.com/gif.latex?OutPut(current)&space;=&space;InOrder(current&space;-&space;1)&space;*&space;ReOrder(current&plus;1)" title="OutPut(current) = InOrder(current - 1) * ReOrder(current+1)" />

其中，ReOrder 数组是可以不必要构建的。我们可以做一个逆序遍历来动态生成current + 1 位置处的ReOrder, 即可满足第二项要求：不使用额外空间。
##代码2：
```python 
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if nums == []:
            return []
        InOrder = [1] * len(nums)
        
        for i in xrange(1,len(nums)):
            InOrder[i] = InOrder[i-1] * nums[i-1]
        temp  = 1
        for i in xrange(1, len(nums) + 1):
            InOrder[-i] = InOrder[-i] * temp
            temp*= nums[-i]
            
        return InOrder
```
##分析
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(1)" style="border:none;">
