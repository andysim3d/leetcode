# 238.Product of Array Except Self

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
$$ 
    F(i) = \frac{(\prod{i=0})}{Num(i)}
$$
