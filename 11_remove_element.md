# 27.Remove Element
[Remove Element](https://leetcode.com/problems/remove-element/)
>Given an array and a value, remove all instances of that value in place and return the new length.


>The order of elements can be changed. It doesn't matter what you leave beyond the new length.



##题目要求：
>给定一个数组和一个值V，移除所有值为V的元素。

##要求：
> in place，即不能新建另外的数组

##解法：
典型的双指针类型，快，慢两个指针同步开始，遇到值为V的元素，快指针向后移动，慢指针不动。否则，将快指针指向的值赋给慢指针，并同步移动快慢指针。

当快指针指向最后的元素时，慢指针即指向移除所有V后的队列的尾部。


##代码：

```python
    class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        if len(nums) < 1:
            return 0
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] == val:
                pass
            else:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```

##分析：

时间复杂度： $$O(n)$$  (**遍历整个数组**)

空间复杂度： $$O(1)$$  (**In place**)