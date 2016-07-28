# Regular Expression Matching

[Delete Node in a Linked List](https://leetcode.com/problems/regular-expression-matching/)

>Implement regular expression matching with support for '.' and '*'.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```


##题目要求：
给定字符串S和正则表达式P，判断P与S是否匹配。其中.匹配任意字符，\*匹配任意多个其前缀字符。
##要求：
无
##解法：
典型的DP问题，对于任意S[i] 和 P[j]可能出现的情况有两种：

- 1 匹配：
  - S[i] == P[j] || P[j] == '.' --> 即DP[i]\[j]应与DP[i - 1]\[j - 1]一致。
- 2 不匹配：
    - P[j] == '\*' :
      - P[j - 1] == S[i] || P[j - 1] == '.'  --> DP[i]\[j] = DP[i - 1]\[j] || DP[i]\[j - 2]
      - j = 0 --> 什么都没发生。
    - P[j] != '\*' :
      - 不匹配

还要注意一点，因为\* 可能匹配到空字串，所以在初始化DP矩阵的时候要注意初始化\*之后的元素可能匹配到S[0]的情况


##代码：
```java

public class Solution {
    /**
     * @param s: A string 
     * @param p: A string includes "." and "*"
     * @return: A boolean
     */
    public boolean isMatch(String s, String p) {
        // write your code here
        boolean [][] DP = new boolean [s.length() + 1][p.length() + 1];
        DP[0][0] = true;
        /////// very important !!!!!
        for (int j = 1; j <= p.length(); j++){
            DP[0][j] = (j > 1) && ('*' == p.charAt(j - 1)) && (DP[0][j - 2]);
        }
        for (int i = 1; i < s.length() + 1; ++i){
            for (int j = 1; j < p.length() + 1; ++j){
                if (p.charAt(j - 1) == '*'){
                    if (j < 2){
                        continue;
                    }
                    DP[i][j] = DP[i][j - 2] 
                        || ((s.charAt(i - 1) == p.charAt(j - 2)) 
                        // pay attention to && DP[i - 1][j]
                        || p.charAt(j - 2) == '.') && DP[i - 1][j];
                }
                else{
                    DP[i][j] = DP[i - 1][j - 1] 
                        && ((s.charAt(i - 1) == p.charAt(j - 1)) 
                        || p.charAt(j - 1) == '.');
                }
            }
            
        }
        return DP[s.length()][p.length()];
    }
}
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n^2)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(n^2)" style="border:none;">
