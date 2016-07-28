# Wildcard Matching

[Regular Expression Matching](https://leetcode.com/problems/wildcard-matching/)

>Implement wildcard pattern matching with support for '?' and '*'.


```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```


##题目要求：
给定字符串S和表达式P，判断P与S是否匹配。其中?匹配任意单个字符，\*匹配任意序列（可以为空）。
##要求：
无
##解法：
贪心问题，对于任意S[i] 与 P[j]，可能出现情况有：

- 1 匹配：
  - S[i] == P[j] || P[j] == '?' --> 此时可以将i与j各向后移一位。
- 2 不匹配：
    - P[j] == '\*' :
        记录j的位置为最近的Star位，并将j + 1（因为\*可以匹配空字符串）
    - P[j] != '\*' :
      - 最近的Star位不为-1，即\*出现过:将j置为 Star + 1,即将不匹配字段全视为由\*匹配。
      - 最近的Star位为-1，即没有出现过\*: 字符串不匹配。

直接扫到S的结尾，**再忽略掉P末尾的\* **,看j是否处于P的尾部，即可知P是否与S相匹配。


##代码：
```java

public class Solution {
    /**
     * @param s: A string 
     * @param p: A string includes "?" and "*"
     * @return: A boolean
     */
    public boolean isMatch(String s, String p) {
        // write your code here
        int lastStar = -1;
        int ps = 0;
        int pp = 0;
        while (ps < s.length()){
            if (pp == p.length()){
                if (lastStar == -1){
                    return false;
                }
                if (pp == lastStar + 1){
                    return true;
                }
            }
            char sval = s.charAt(ps);
            char pval = p.charAt(pp);
            // if match 
            // or if p match all
            if (sval == pval || pval == '?'){
                ps ++;
                pp ++;
                continue;
            }
            // if p is *, mark and rematch(ignore *)
            if (pval == '*'){
                lastStar = pp;
                pp ++;
                continue;
            }
            // if not all those conditions
            else{
                // if no star occurs
                if (lastStar == -1){
                    return false;
                }
                else{
                    //jump back to lastStar
                    pp = lastStar + 1;
                    // and move ps pointer one step
                    ps ++;
                    continue;
                }
            }
        }
        while (pp < p.length()){
            if (p.charAt(pp) == '*'){
                pp ++;
            }
            else{
                break;
            }
        }
        return pp == p.length();
     }
}
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(1)" style="border:none;">
