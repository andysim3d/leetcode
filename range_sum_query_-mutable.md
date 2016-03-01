# 307. Range Sum Query -Mutable
[Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)

>Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

>The update(i, val) function modifies nums by updating the element at index i to val.

>Example:

>```
Given nums = [1, 3, 5]
sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
>Note:
>The array is only modifiable by the update function.
You may assume the number of calls to update and sumRange function is distributed evenly.

##题目要求：
给定一个数组，实现sumRange()和update()两个操作。

##要求：
两种操作被调用的频率几乎一致。

##解答：
这题用单纯的数组和会导致时间过长。所以需要使用高级数据结构。选择有两种，线段树和树状数组。


###树状数组：


```python
class NumArray(object):
    def __init__(self, nums):
        """
        initialize your data structure here.
        :type nums: List[int]
        """
        self.sums = [0] * (len(nums) + 1)
        self.nums = nums
        self.n = len(nums)
        for i in xrange(len(nums)):
            self.add(i + 1, nums[i])
            
    def add(self, x, val):
        while x <= self.n:
            self.sums[x] += val
            x += self.lowbit(x)
            
    def sum(self, x):
        res = 0
        while x > 0:
            res += self.sums[x]
            x -= self.lowbit(x)
        return res
    
    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: int
        """
        self.add(i + 1, val - self.nums[i])
        self.nums[i] = val

    def sumRange(self, i, j):
        """
        sum of elements nums[i..j], inclusive.
        :type i: int
        :type j: int
        :rtype: int
        """
        if not self.nums:
            return 0
        return self.sum(j+1) - self.sum(i)
        
    def lowbit(self, x):
        return x & (-x)

```

##分析：
时间复杂度：

update:

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(log(n)) " style="border:none;">

sumarrang:

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(log(n)) " style="border:none;">


空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">
