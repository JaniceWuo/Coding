# leetcode97 交错字符串
题目链接：https://leetcode-cn.com/problems/interleaving-string/
## 题目描述
给定三个字符串 s1, s2, s3, 验证 s3 是否是由 s1 和 s2 交错组成的。<br/>
输入: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"   输出：true

## 思想
如果遍历字符串，考虑s3是否包含s1，s3去掉s1后是否等于s2的话，会导致s3在去掉s1的字母时会去掉先出现的那个字母，比如s3-s1="dbbac"!=s2，会输出False。<br/>
所以要用动态规划：dp[i][j]表示s1的前i个字符和s2的前j个字符能否组成s3的前i+j个字符，有dp[0][0]=True，先计算出边界：dp[0][]和dp[][0]的各值。
## code
```Python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        n1 = len(s1)
        n2 = len(s2)
        n3 = len(s3)
        if n3!=n1+n2:
            return False
        dp = [[False]*(n2+1) for _ in range(n1+1)]
        dp[0][0] = True
        for i in range(1,n1+1):
            dp[i][0] = dp[i-1][0] and s1[i-1]==s3[i-1]
        for j in range(1,n2+1):
            dp[0][j] = dp[0][j-1] and s2[j-1]==s3[j-1]
        for i in range(1,n1+1):
            for j in range(1,n2+1):
                dp[i][j] = (dp[i][j-1] and s2[j-1]==s3[i+j-1]) or (dp[i-1][j] and s1[i-1]==s3[i+j-1])
        return dp[n1][n2] 
```