# BackPack
[Backpack](https://leetcode.com/problems/backpack/)


>Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?
>
##Example:
If we have 4 items with size [2, 3, 5, 7], the backpack size is 11, we can select [2, 3, 5], so that the max size we can fill this backpack is 10. If the backpack size is 12. we can select [2, 3, 7] so that we can fulfill the backpack.
>
You function should return the max size we can fill in the given backpack.


##题目要求：
给定数组，每个元素表示一个物体的重量，求，如何组合可以最大限度接近背包极限容量？
##要求：
O(nm) 时间 和 O(m) 空间复杂度.

##解答：
动态规划类题目。经典背包问题。可以考虑，假设当前背包容量为 n, 已经被放置了i个物体，最远能放到m。那么对第i+1个物体，有着:
```
F(k + w(i + 1)) = | true (if F(k) == true);
                  | false ( if F(k) == false);
                  (0 <= k <= m)
```
最后，只要找满足 F(k) == true 的最大k值，即为能够放置重量最多的组合。


##代码：

Java:
```Java
public class Solution {
  public class Solution {
      /**
       * @param m: An integer m denotes the size of a backpack
       * @param A: Given n items with size A[i]
       * @return: The maximum size
       */
      public int backPack(int m, int[] A) {
          // write your code here
          int [] pack = new int[m + 1];
          pack[0] = 1;
          int max = 0;
          for (int i = 0; i < A.length ; ++i){
              for (int j = m; j >= 0; --j){

                  if (pack[j] == 1 && j + A[i] <= m){

                      pack[j + A[i]] = 1;
                      max = Math.max(j + A[i], max);
                  }
              }
              // for ( int d : pack){
              //     System.out.print(d);
              //     System.out.print(", ");

              // }
              // System.out.println();
          }
          return max;
      }
  }
```
##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(n * m) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(m) " style="border:none;">

# Backpack II
[Backpack II](http://www.lintcode.com/en/problem/backpack-ii/)

>Given n items with size Ai and value Vi, and a backpack with size m. What is the maximum value can you put into the backpack?
>
##Example
>
Given ```4``` items with size``` [2, 3, 5, 7]``` and value ```[1, 5, 2, 4]```, and a backpack with size ```10```. The maximum value is ```9```.

##题目要求：
和I的要求类似，只是这次不仅有不同重量，还有不同的价值。我们要求计算最多能拿到的价值。

##要求：
O(m)空间复杂度。

##解答：
这题同理I，对于任意时刻，假设我们要尝试第i个元素，此前最多能取到的重量组合为m，最大价值为V，则有：
```
F(k + weight(m)) = |- Max(F(k) + value(m), F(k + weight(m)))
                   | if F(k) != 0
                   |- 0.
                   (0 =< k <= m)
```
然后同时用一个变量max记录在计算中会出现的最大值。尝试过所有的元素后，返回出现过的最大值。

##代码：

```Java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A & V: Given n items with size A[i] and value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int V[]) {
        // write your code here
        int [] pack = new int[m + 1];
        pack[0] = 1;
        int max = 0;
        for (int i = 0; i < A.length ; ++i){
            for (int j = m; j >= 0; --j){

                if (pack[j] != 0 && j + A[i] <= m){

                    pack[j + A[i]] = Math.max( pack[j + A[i]], pack[j] + V[i]);
                    max = Math.max(pack[j] + V[i], max);
                }
            }
            // for ( int d : pack){
            //     System.out.print(d);
            //     System.out.print(", ");

            // }
            // System.out.println();
        }
        return max - 1;

    }
}
```

##分析：
时间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(nm) " style="border:none;">

空间复杂度：

<img src="http://chart.googleapis.com/chart?cht=tx&amp;chl=\Large O(m) " style="border:none;">
