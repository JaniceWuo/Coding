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
这种用dp的方法时间负责度为O(n^2)。利用贪心加二搜可以降到O(nlogn)。<br/>
用d[len]表示长度为len的上升子序列末尾的最小元素。比如对于初始数组[1,5,2,3]，有d[1]=1,d[2]=5,但是遍历到2时，发现2比5小，此时可以组成上升序列[1,2]，所以d[2]变为2，然后d[3]=3，则数组d=[1,2,3]。（d记录的并不是真正的上升序列，而是长度为len时的末尾最小元素。）<br/>
d数组肯定是单调递增的，因为长度为2的上升子序列的末尾`最小元素`肯定比长度为3的上升子序列的末尾`最小元素`小，可以用反证法证明单调递增。当遍历到的当前数nums[i]比d[-1]要小时，则要去d数组中找到`第一个`比它大的数，比如d=[1,4,6]，当此时的nums[i]=3时，d要变为[1,3,6]，表示长度为2的上升子序列末尾最小元素应该变为3而不是4。<br/>
我们只能用d最后的长度而不能用d来表示子序列。比如对于给定数组[1,4,6,3]，我们最终得到的d=[1,3,6]，但是一眼就能看出给定数组的最长上升子序列应该为[1,4,6]。就算长度为2的末尾最小元素更新为了3，长度为3的最小元素依然是6，没有改变。
```Python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 0:
            return 0
        if n == 1:
            return 1
        d = [nums[0]]
        for i in range(1,n):
            if nums[i] > d[-1]:
                d.append(nums[i])
            else:
                left = 0
                right = len(d)-1
                loc = right 
                while left<=right:
                    mid = (left+right)//2
                    if d[mid] >= nums[i]:
                        loc = mid
                        right = mid-1
                    else:
                        left = mid+1
                d[loc] = nums[i]
        return len(d)
```