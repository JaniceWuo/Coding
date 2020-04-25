## 题目
给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。

## 思考
注意：子序列不需要连续，子串需要连续。<br/>
此题用dp，dp[i]表示前i个数并且以nums[i]结尾的最长上升序列的长度。要遍历前i-1个数，用j来遍历，只有当nums[j] < nums[i]时才能组成上升序列，此时dp[i] = dp[j] + 1。要取最大。

## code
```Python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 0:
            return 0
        if n == 1:
            return 1
        dp = [1]*n
        for i in range(1,n):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i],dp[j] + 1)
        return max(dp)
```