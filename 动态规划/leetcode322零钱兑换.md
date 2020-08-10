## 题目
有不同面额的硬币coins和一个总金额amount。求凑成总金额所需最少硬币数，每种硬币数量无限。如果不能组成，返回-1。

## 思考
看到这题的时候，第一想法是先用面值最大的硬币去凑，然后剩余的钱再去匹配较小面额的硬币，这样确实能保证是最少硬币数，但是每次都用大面额的硬币，并不能保证这样做最后刚好能够凑出总金额，最后错误的返回-1，其实可能用较小面额的是能得到总金额的。<br/>
所以应该用动态规划而不是上面的贪心思想。
动态规划也有两种：第一种是总金额由1变为amount，然后每次都遍历coins。
第二种是外循环是coins，内循环金额由硬币币值开始遍历直到amount。

## code
第一种：
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        Arrays.sort(coins);  //其实也可以不排序orz
        int[] dp = new int[amount + 1];
        Arrays.fill(dp,amount + 1);  //填充为amount+1是因为最多也不可能超过amount枚硬币
        dp[0] = 0;
        for(int i = 1; i <= amount; i++){
            for(int j = 0; j < coins.length; j++){
                if(coins[j] <= i){
                    dp[i] = Math.min(dp[i],dp[i-coins[j]] + 1); //注意dp[i]不需要加一
                }else{
                    break;
                }
            }
        }
        return dp[amount] > amount?-1:dp[amount];   
    }
}
```
第二种： （这种运行时间稍微快点）
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp,amount + 1);
        dp[0] = 0;
        for(int coin:coins){
            for(int i = coin; i <= amount;i++){
                dp[i] = Math.min(dp[i],dp[i-coin] + 1);
            }
        }
        return dp[amount] > amount?-1:dp[amount];   
    }
}
```