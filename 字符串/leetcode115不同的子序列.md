## 题目
给定一个target串：T  source串：S
计算在S的子序列中T出现的个数。
（子序列不要求连续，可以删除一些也可以不删除字符）
比如s = "rabbbit"  t = "rabbit"  则输出3。

## 思考
这题感觉挺难的。因为删除字符这关，可以删除也可以不删除，如果去讨论每个字符删还是不删就陷入死胡同了。
另dp[i][j]表示为s的前j个字符的子序列有多少个等于t的前i个字符。有dp[0][j]=1，因为空串是任意字符串的一个子序列，包括空串。也有dp[1][0]、d[2][0]...都为0,因为任意非空字符串都不可能是空串的子序列。
当s[j-1]==t[i-1]，dp[i][j] = dp[i-1][j-1] + dp[i][j-1],解释：比如s="baa" t="ba",当都遍历到最后一个字符"a"的时候，是相等的，此时应当看t的"b"在s的"ba"的子序列中有多少个匹配的，以及t的"ba"在s的"ba"的子序列有多少个匹配的。因为虽然当前字符相等，s的前面的字符也可能有和t当前遍历的字符相等的。
当s[j-1]!=t[i-1]，当前字符都不相等了，那就一定要带上t的当前字符去和s的前j-1个字符比较：dp[i][j] = dp[i][j-1]。

## code
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[n+1][m+1];  //因为有空串
        for(int j = 0; j <= m; j++){
            dp[0][j] = 1;
        }
        for(int i = 1;i <= n; i++){
            for(int j = 1;j <= m; j++){
                if(s.charAt(j-1) == t.charAt(i-1)){
                    dp[i][j] = dp[i-1][j-1] + dp[i][j-1];
                }else{
                    dp[i][j] = dp[i][j-1];
                }
            }
        }
        return dp[n][m];
    }
}
```

