# Delete Node in a Linked List

[Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

>Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

>Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

##题目要求：
给你以一个链表中某个节点，要求你删除当前节点

##要求：
无
##解法：
对于此题，一开始觉得无从下手：如果是双向链表，我还能知道前一个元素是什么，单链表怎么知道前一个元素是什么呢？

不过转换一下思路，如果我将当前节点变成当前节点的下一个节点，那是不是就相当于删除了当前节点？

例：

1-> 2 -> 3 -> 4 -> 5

1-> 2 -> 4 - ~~| 4 |~~ -> 5 

这样，我们就相当于是删除了节点3

##代码：
```python

# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next

```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(1)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(1)" style="border:none;">
