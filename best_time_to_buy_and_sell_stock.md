# 121. Best Time to Buy and Sell Stock
[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

>Say you have an array for which the ith element is the price of a given stock on day i.

>If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

##题目要求：
给定一个数组，为股票价格。在只能交易一次的情况下，计算出最大收益

##要求：
无

##解答：
可以将问题转化为求[最大子序列和]()，只要参数为当天与前一天股票的差值即可。


##代码：

```python

class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if prices == [] or len(prices) == 1:
            return 0
            
        #此处用的为生成器，并非开新数组
        p = (prices[i + 1] - prices[i] for i in xrange(len(prices) - 1))
        RealMax = 0
        Max = 0
        for i in p:
            Max = max(i, Max + i)
            RealMax = max(RealMax, Max)
            
        return RealMax
```
##分析：

时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">






# 122. Best Time to Buy and Sell Stock II

[Best Time to Buy and Sell Stock II ](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
>Say you have an array for which the ith element is the price of a given stock on day i.

>Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

##题目要求：
和上一题极其类似，而且简单了好多，不限制交易次数。

##要求：
无
##解答：
简单来说，求当天与前一天的差值，并加和所有大于0的结果（即有收益的结果），就可以得到答案


##代码：
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if prices == [] or len(prices) == 1:
            return 0
            
        #此处用的为生成器，并非开新数组
        p = (prices[i + 1] - prices[i] for i in xrange(len(prices) - 1))
        return sum(i for i in p if i > 0)
        ```

##分析：

时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(1) " style="border:none;">