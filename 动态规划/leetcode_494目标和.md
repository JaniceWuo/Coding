## 题目描述
给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。
返回可以使最终数组和为目标数 S 的所有添加符号的方法数。<br/>
输入: nums: [1, 1, 1, 1, 1], S: 3 输出：5<br/>
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/target-sum

## 思考
如果总是去想这个正负号问题，可能就会陷入瓶颈。<br/>
其实添加+和-就是从nums中选出一部分数字变为负数，其他仍是正数，然后求其和。<br/>
当数组全为正时，和为sum(nums)，若把一个数num变为负数，则和变为sum(nums)-2 * num。设变为负数的数有m个，则S = sum(nums)-2 * (这m个数的和)，这m个数的和一定是个整数，所以(sum(nums)-S)/2一定为整数，则(sum(nums)-S)%2必须为0，若不为0则没有方法数。而且若sum(nums)比目标S要小，则方法数也为0。问题转化为求一个数组nums中，挑选出m个数，使其和为(sum(nums)-S)//2。

## code
```Python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        #转换为01背包  将nums划分为正数集和负数集
        n = len(nums)
        if sum(nums)<S or (sum(nums)-S)%2!=0:
            return 0
        target = (sum(nums)-S)//2
        dp = [0]*(target+1)
        dp[0] = 1
        #要掌握01背包的一维解  不然可能会超时
        for num in nums:
            for i in range(target,num-1,-1):
                dp[i] = dp[i] + dp[i-num]
        return dp[target]
```
