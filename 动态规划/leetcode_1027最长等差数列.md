## 题目
给定一个整数数组 A，返回 A 中最长等差子序列的长度。

回想一下，A 的子序列是列表 A[i_1], A[i_2], ..., A[i_k] 其中 0 <= i_1 < i_2 < ... < i_k <= A.length - 1。并且如果 B[i+1] - B[i]( 0<=i<B.length-1) 的值都相同，那么序列 B 是等差的。

输入：[9,4,7,2,10]
输出：3
解释：
最长的等差子序列是 [4,7,10]。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-arithmetic-sequence


## 思考
运用dp，dp里面存的是dict，dict{差值：长度}。比如对于[9,4,7,2,10]，dp[0]为空，dp[1] = {4-9:2} = {-5:2}，意思是等差为-5的长度为2。dp[2] = {-2:2},dp[3] = {3:2}。以此类推。<br/>
意思就是说当前数nums[i]和前面遍历到的数nums[j]的差如果为d，则去找dp[j]中差为d的等差数列长，则dp[i]差为d的长为dp[j]中差为d的等差数列长加1，如果没找到的话为2。

## code
```Python
class Solution:
    def longestArithSeqLength(self, A: List[int]) -> int:
        n = len(A)
        dp = [{} for _ in range(n)]
        res = 0
        for i in range(1,n):
            for j in range(i):
                if j == 0:
                    dp[i][A[i]-A[j]] = 2
                else:
                    dis = A[i] - A[j]
                    x = dp[j].get(dis,1) + 1   #dict.get(键：默认值) 如果找不到该键则赋值默认值
                    dp[i][dis] = x
            res = max(res,max(dp[i].values()))
        return res
```