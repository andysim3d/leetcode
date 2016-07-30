# Scramble String

[Scramble String](http://www.lintcode.com/en/problem/scramble-string/)

>Given a string ```s1```, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.
>
Below is one possible representation of ```s1 = "great"```:
```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
To scramble the string, we may choose any non-leaf node and swap its two children.
>
For example, if we choose the node ```"gr"``` and swap its two children, it produces a scrambled string ```"rgeat"```.
```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```
We say that ```"rgeat"``` is a scrambled string of ```"great"```.
>
Similarly, if we continue to swap the children of nodes ```"eat"``` and ```"at"```, it produces a scrambled string ```"rgtae"```.
```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```
We say that ```"rgtae"``` is a scrambled string of ```"great"```.
>
Given two strings s1 and s2 of the same length, determine if s2 is a scrambled string of s1.

####Example
```"great", "rgtae" -> true```


##题目要求：
首先给出了一个叫Scramble string的定义，即给定一个String，我们可以将它分解为任意二叉树形式。 比如给出 ```"great"```，可以分解成：
```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
然后，我们可以进行任意多次的旋转任意子树，比如可以旋转```”gr“```，使它变为 ```"rg"```。

我们可以重复上面的步骤多次，得到的String与原String互为Scramble String.

现在，给定String A和 B，要判断A与B是不是 Scramble String。

##要求：
时间复杂度为 O($$n^3$$)
##解法：
这种题目看起来毫无思路，但经过分析后我们可以考虑，可以用DP的方式。

如果 S1 和S2互为 Scramble String，那么一定存在i位置，可以将S1 分为 S11，S12，j位置S2分为 S21，S22，使 S11/S12 与 S21/S22 互为 Scramble String。（可能出现的组合有两种，[S11<->S21, S12 <-> S22],[S11 <-> S22, S12 <-> S21]）。

假设 DP[len][i][j] 为长度为len的字符串是否可以用s1[i] 和 s1[len - i] ，s2[j], s2[len - j] 组成 Scramble String.

状态转移方程为：

```DP[len][i][j] = for k in 1 ... len : 
         DP[k][i][j] 
      && DP[len - k][i + k][j + k]
      || DP[len - k][i + k][j] 
      && DP[k][i][j + len - k]```
然后最后判断 DP[length][0][0] 是否为true。（取0 是因为不需要考虑子串了）。


##代码：

```java

public class Solution {
    /**
     * @param s1 A string
     * @param s2 Another string
     * @return whether s2 is a scrambled string of s1
     */
    public boolean isScramble(String s1, String s2) {
        // Write your code here
        if (s1 == null && s2 == null){
            return true;
        }
        if (s1.length() != s2.length()){
            return false;
        }
        if (s1.equals(s2)){
            return true;
        }
        int length = s1.length();
        boolean [] [] [] DP = new boolean [length + 1][length ][length ];
        for (int i = 0; i < length ; ++i){
            for (int j = 0; j < length ; ++j){
                if (s1.charAt(i) == s2.charAt(j)){
                    DP[1][i][j] = true;
                }
            }
        }
        for (int len = 2; len < length + 1; ++len){
            for (int i = 0; i < length - len + 1 ; ++i){
                for (int j = 0; j < length - len + 1 ; ++j){
                    for (int k = 1; k < len; ++k){
                        if (DP[len][i][j]){
                            break;
                        }
                        DP[len][i][j] |= DP[k][i][j] && DP[len - k][i + k][j + k]
                            || DP[div][i][j + len - k] && DP[len - k][i + k][j];
                    }
                }
            }
        }
        return DP[length][0][0];
    }
}
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n^3)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(n^3)" style="border:none;">

