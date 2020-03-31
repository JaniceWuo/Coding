# 编辑距离
## 题目描述
给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。（增、删、替）<br/>
## 思想
肯定要用dp解答，关键是dp的定义：dp[i][j]为word1的前i个词转换为word2的前j个词的最少次数。和01背包类似，要讨论当前这两个词是要进行改动还是不用动，即考虑word[i-1]是否等于word2[j-1]。<br/>
若word1[i-1]==word2[j-1]，则最后一个词不用改动，此时dp[i][j] = dp[i-1][j-1]；若不等，则找到min值然后要加1。<br/>
## 代码
```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n = len(word1)
        m = len(word2)
        if n==0 or m ==0:  #要先判断一下两个字符串中有没有空字符串，起到一定减少运行时间的作用
            return n+m
        dp = [[0]*(m+1) for _ in range(n+1)]
        for i in range(n+1):
            dp[i][0] = i
        for j in range(m+1):
            dp[0][j] = j
        
        for i in range(1,n+1):
            for j in range(1,m+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1])+1
        return dp[n][m]
```
