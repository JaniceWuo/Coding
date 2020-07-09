您可以在某些特定的城市和星期休假。您的工作就是安排旅行使得最大化你可以休假的天数，但是您需要遵守一些规则和限制。

规则和限制：

您只能在 N 个城市之间旅行，用 0 到 N-1 的索引表示。一开始，您在索引为0的城市，并且那天是星期一。
这些城市通过航班相连。这些航班用 N*N 矩阵 flights（不一定是对称的）表示，flights[i][j] 代表城市i到城市j的航空状态。如果没有城市i到城市j的航班，flights[i][j] = 0；否则，flights[i][j] = 1。同时，对于所有的i，flights[i][i] = 0。
您总共有 K 周（每周7天）的时间旅行。您每天最多只能乘坐一次航班，并且只能在每周的星期一上午乘坐航班。由于飞行时间很短，我们不考虑飞行时间的影响。
对于每个城市，不同的星期您休假天数是不同的，给定一个 N*K 矩阵 days 代表这种限制，days[i][j] 代表您在第j个星期在城市i能休假的最长天数。
给定 flights 矩阵和 days 矩阵，您需要输出 K 周内可以休假的最长天数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-vacation-days
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


## 思考
另dp[i][k]表示从第k周开始，且第k周起始点为i城（可以理解为周一早上在i城或者说是上一周在i城玩所以周一早上还在i城，上午可以选择乘坐航班去其他城市)的最大休假天数。那么dp[0][0]为题目要求的最长休假天数。
那么第k周可以就选择在i城度过（dp[i][week]=days[i][week]+dp[i][week+1]，也可以飞到其他城市去比如j城(days[j][week]+dp[j][week+1])。

## code
```java
class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        if(flights.length == 0||days.length==0) return 0;
        int[][] dp = new int[days.length][days[0].length + 1]; //这里week的内存空间多1  是因为要计算week+1
        for(int week = days[0].length - 1;week>=0;week--){
            for(int i = 0;i<days.length;i++){
                dp[i][week] = days[i][week] + dp[i][week+1];
                for(int j = 0;j<flights[0].length;j++){
                    if(flights[i][j] == 1) dp[i][week] = Math.max(dp[i][week],days[j][week] + dp[j][week+1]);
                }
            }
        }
        return dp[0][0];
    }
}
```
就是如果是当前的dp需要用到后面dp的结果，并且最后是返回dp[0][0] 那循环的时候应当有一个是倒序。