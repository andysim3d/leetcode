# Copy Books

[Copy Books](http://www.lintcode.com/en/problem/copy-books/)

>Given an array A of integer with size of n( means n books and number of pages of each book) and k people to copy the book. You must distribute the continuous id books to one people to copy. (You can give book A[1],A[2] to one people, but you cannot give book A[1], A[3] to one people, because book A[1] and A[3] is not continuous.) Each person have can copy one page per minute. Return the number of smallest minutes need to copy all the books.


####Example
>Given array A = ```[3,2,4]```, k = ```2```.
>
Return ```5```( First person spends 5 minutes to copy book 1 and book 2 and second person spends 4 minutes to copy book 3. )



##题目要求：
给出一组数列pages，表示抄写一系列书所需要的时间。并给定k个人，每个人抄写连续的某几本书，求怎样分配后总的抄写时间最少。

你可以将 1，2 两本书交由同一人抄写，但你不能将1，3交由同一人，因为1，3不连续。

##要求：
无
##解法：
还是DP的问题。假设我们有数组DP[]\[],对于 DP[i]\[j],表示 i个人抄j本书所需的最少时间。可以得到 DP[1]\[j]的全部解。递推，可知 对于任意 DP[i]\[j]，有状态转移方程：

```
DP[i][j] = min(DP[i - 1][j],  // 完全不用第i个人抄写 
            max(DP[i - 1][j - 1], page[j]),  //让第i个人抄写第j份
            max(DP[i - 1][j - 2], page[j] + page[j - 1]),  //让第i个人抄写j, j - 1两份。
            ...,
            max(DP[i - 1][0], page[j] + page[j - 1] + ... + page[0]))//全部让第i个人写。
```
可解得最后 DP[k]\[pages.length]即为 k人抄所有书所需最短时间。

这里有一个可以优化的地方，因为对于 ```DP[1][j]``` 来说，它就是SUM(```pages[0 ... j]```)。所以我们计算在   ```pages[j] + pages[j - 1] + ... + pages[n]```   时，可以用 ```DP[1][j] - DP[1][n]```来代替。



##代码：

```java

public class Solution {
    /**
     * @param pages: an array of integers
     * @param k: an integer
     * @return: an integer
     */
    public int copyBooks(int[] pages, int k) {
        // write your code here
        // DP[i][j] --> i person j pages, minimum time
        int [][] DP = new int[k + 1][pages.length + 1];
        DP[1][1] = pages[0];
        for (int i = 1; i < pages.length + 1; ++i){
            DP[1][i] = DP[1][i - 1] + pages[i - 1];
        }
        // BTW, DP[1][j] = SUM[j - 1](pages)
        
        for (int i = 2; i < k + 1; ++i){
            for (int j = pages.length; j >= 0; -- j){
                //suppose this guy gain nothing;
                DP[i][j] = DP[i - 1][j];
                //then from [l] to [l, 0], try to match min
                for (int l = j; l > 0; --l){
                    DP[i][j] = Math.min(DP[i][j], 
                                  Math.max(DP[i - 1][l] , DP[1][j] - DP[1][l]) );
                }
            }
        }
        return DP[k][pages.length];
    }
```
##分析：
时间复杂度：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large  O(n^3)" style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large O(n^2)" style="border:none;">
