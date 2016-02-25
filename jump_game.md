# 55. Jump Game

[Jump Game](https://leetcode.com/problems/jump-game/)
>Given an array of non-negative integers, you are initially positioned at the first index of the array.

>Each element in the array represents your maximum jump length at that position.

>Determine if you are able to reach the last index.

>For example:

>A = ```[2,3,1,1,4]```, return ```true```.

>A = ```[3,2,1,0,4]```, return ```false```.

##题目要求：
给定一个数组，每个数字代表从其位置可以向后移动多少步。计算是否能移动到最后一块。

##要求
无

##解答：

经典的贪心算法问题。对于这类题，首先要确定当前能跳跃到的最大范围，然后继续由该范围中的所有值为输入，求能跳到的最远距离。直到能跳到的最大范围为空集（即跳不动了）或跳到最后一块为止

##代码：

```python
class Solution(object):
    def canJump(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        if nums == []:
            return 0
            
        maxi = nums[0]
        tmpmaxi = nums[0]
        for i, num in enumerate(nums):
            if i > maxi:
                if maxi == tmpmaxi:
                    return False
                else:
                    maxi = tmpmaxi
            tmpmaxi = max(tmpmaxi, i + num)
        
        return True
```


##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">



# 45. Jump Game II

[Jump Game II](https://leetcode.com/problems/jump-game-ii/)


>Given an array of non-negative integers, you are initially positioned at the first index of the array.

>Each element in the array represents your maximum jump length at that position.

>Your goal is to reach the last index in the minimum number of jumps.
>
For example:
Given array A = ```[2,3,1,1,4]```
>
The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

##题目要求：
和 I 一样的要求，只是这回需要返回最少跳几次可以到达。

##要求：
无

##解答：
和1一样的解法，记得在每次切换跳跃范围时记得计数就可以了

##代码：

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        if len(nums) <= 1:
            return 0
        count = 1
        maxi = nums[0]
        tmpMax = 0
        for i, num in enumerate(nums):
            if i > maxi:
                count += 1
                maxi = tmpMax
            tmpMax = max(tmpMax, i + num)
        
        return count
```



##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">