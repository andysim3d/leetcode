# 41.First Missing Positive My Submissions Question
[First Missing Positive My Submissions Question](https://leetcode.com/problems/first-missing-positive/)

>Given an unsorted integer array, find the first missing positive integer.
>
For example,
Given ```[1,2,0]``` return ```3```,
and ```[3,4,-1,1]``` return ```2```.
>
Your algorithm should run in O(n) time and uses constant space.

##题目要求：
给定乱序数列，要求找到缺少的第一个正数

例如给定```[1,2,0]```，缺少第一个正数就是3

##要求：
不能使用额外空间，O(n)的时间复杂度

##解答：
如果没有限制不能使用额外空间的话，这题就是简单的用散列表/哈希表就可以解决的问题。

不过因为有限制条件，我们需要考虑，能否不使用额外空间。

可以考虑，我们将输入的数组视为哈希表，数组下标+1表示其值。那么，我们需要先遍历数组一次，将所有可以达成 *下标+1 = 值* 的元素放置在相应的位置。然后从第一个元素开始遍历，第一个不满足
*下标 + 1 = 值* 的元素即为缺少的第一个正数。


##代码：

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 1

        i = 0
        # resort them
        while i < len(nums):
            if nums[i] != i + 1                     \
                and nums[i] >= 1                    \
                and nums[i] <= len(nums)            \
                and nums[nums[i] - 1] != nums[i] :
                #必须用临时变量记录，否则会死循环。
                tmp = nums[i] - 1
                nums[i], nums[tmp] = nums[tmp], nums[i]
            else:
                i += 1


        for i in xrange(len(nums)):
            if nums[i] != i+1:
                return i + 1
        return len(nums) + 1
            
            

```

##分析
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">
