# Binary Search Tree Iterator

[Binary Search Tree Iterator](https://leetcode.com/problems/regular-expression-matching/)

>Design an iterator over a binary search tree with the following rules:
>
Elements are visited in ascending order (i.e. an in-order traversal)
next() and hasNext() queries run in O(1) time in average.


##题目要求：
设计一个迭代器，要求用该迭代器可以遍历二叉搜索树的所有元素。尽量使迭代器的 next() 和hasNext()方法时间复杂度为O(1)
##要求：
进阶：O(h) 空间复杂度内解决
终极：O(1) 时间复杂度内解决。
##解法：
如果要求为O(h)的话，我们可以：
对于每一个访问中的节点，都用栈来记录。

如果要求为O(1)的话，解法就是：使用 **[线索二叉树](https://en.wikipedia.org/wiki/Threaded_binary_tree)**。

这里我们不构造完全线索二叉树，因为对于该题，左孩子线索化没有必要，我们只要线索化右孩子就可以得到类似链表结构的线索二叉树。

将树线索话可以使用递归，从根节点开始，传入一个祖先节点和当前节点。

- 如果当前节点左孩子不为空，则将当前节点作为祖先，和左孩子一起进行下一轮递归。
- 如果当前节点右孩子不为空，则将祖先节点和右孩子一起进行下一轮递归
- 如果当前节点右孩子为空，将当前节点的右孩子指向祖先

这样完成二叉搜索树的线索化之后，就可以从任意节点遍历所有大于它的节点。

PS：注意处理根节点右孩子为空的情况。这里我使用一个prev 和current记录上一个状态和当前状态。

PSS:注意当从左子树扫到右子树时，一定要找到右子树的最小值，否则会缺少值。

##代码：
```java

/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 * Example of iterate a tree:
 * BSTIterator iterator = new BSTIterator(root);
 * while (iterator.hasNext()) {
 *    TreeNode node = iterator.next();
 *    do something for node
 * } 
 */
public class BSTIterator {
    //@param root: The root of binary tree.
    TreeNode current = null;
    TreeNode root = null;
    TreeNode prev = null;
    Stack<TreeNode> pk = new Stack<TreeNode>();
    public BSTIterator(TreeNode root) {
        // write your code here
        if (root == null){
            return;
        }
        this.root = root;
        inOrder(root, root);
        current = root;
        if (root.right == root){
            root.right = null;
        }
        while(current.left != null){
            current = current.left;
        }
    }
    
    private void inOrder(TreeNode root, TreeNode current){
        if (root == null){
            return;
        }
        if (current.left != null){
            inOrder(current, current.left);
        }
        if (current.right != null){
            inOrder(root, current.right);
        }
        else{
            current.right = root;
        }
    }

    //@return: True if there has next node, or false
    public boolean hasNext() {
        if (current == null){
            return false;
        }
        // use prev to save previous state
        if (current == prev){
            return false;
        }
        // if prev == current, means there is a cycle and you have visit this node.
        if (prev != null && current == this.root && prev.val > this.root.val){
            return false;
        }
        return true;
    }
    
    //@return: return next node
    public TreeNode next() {
        prev = current;
        current = current.right;
        // if current has left, and left.val > prev.val, means you have visited right child of tree,
        // so have to find the smallest value (most left element)
        if (current != null && current.left != null && current.left.val > prev.val){
            while(current.left != null){
                current = current.left;
            }
        }
        return prev;
    }
}
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(1)" style="border:none;">
