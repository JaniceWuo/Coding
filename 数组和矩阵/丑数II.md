## 题目
编写一个程序，找出第 n 个丑数。
丑数就是质因数只包含 2, 3, 5 的正整数。
https://leetcode-cn.com/problems/ugly-number-ii/

## 思考
如果是暴力枚举会超时。技巧：定义三个index，分别表示当前下标的数乘2，乘3，乘5。因为其实要按大小排序，所以选下一个丑数的时候就要比较，选最小的作为下一个丑数。丑数初始为1，下一个丑数就在（1 * 2，1 * 3,1 * 5）中选最小，所以为2。由于1已经与2乘了，所以就在（2 * 2,1 * 3,1 * 5）中选最小，为3。以此类推。

## code
```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int a = 0,b = 0,c = 0;  //三个下标 初始化均为0
        for(int i = 1;i<n;i++){
            int num2 = dp[a] * 2;
            int num3 = dp[b] * 3;
            int num5 = dp[c] * 5;
            dp[i] = Math.min(num2,Math.min(num3,num5)); //三个数中选最小
            if(dp[i] == num2) a++;  //如果是乘2的结果选为下一个丑数  则a++ 表示当前的丑数不能再与2相乘了
            if(dp[i] == num3) b++;
            if(dp[i] == num5) c++;
        }
        return dp[n-1];
    }
}
```

其实也是好理解的。就是要保证选最小，而且还不能用到已经选为丑数的结果，所以指针要++。