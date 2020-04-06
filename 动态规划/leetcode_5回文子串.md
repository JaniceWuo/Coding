## 题目描述
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。<br/>
示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。<br/>
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-palindromic-substring

## 思考
暴力法会超时，考虑用dp。<br/>
首先若s只有一个数，则返回s。要知道一个回文串掐头去尾后仍然是回文串。<br/>
用i,j分别表示一个串的起始和终止位置，s[i]==s[j]时才有必要去掐头去尾查看内部串。因为题目要求最长，所以要保存满足条件的起始结束位置，计算长度。

## 代码
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        if n == 1:
            return s
        dp = [[False]*n for _ in range(n)]
        for i in range(n):
            dp[i][i] = True
        PrevNum = 0
        AfterNum = 0
        maxNum = 1
        for j in range(1,n):   #技巧  为什么把j放前面  因为状态转移公式是dp[i][j] = dp[i+1][j-1],计算i要用到i+1
            for i in range(j):
                if s[i]!=s[j]:
                    continue
                else:
                    if j-i <=2:  #满足s[i]==s[j]情况下，串的总长度为2或者为3都是回文串，如aa、aba
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i+1][j-1]  #计算j用到j-1 所以循环把j放第一
                if dp[i][j]:
                    if j-i+1 > maxNum:
                        maxNum = j-i+1
                        PrevNum = i
                        AfterNum = j
        return s[PrevNum:AfterNum+1]
```

如果一开始dp存的是每个回文串的长度，不行。这样没办法通过状态转移方程检查子串是不是回文串。