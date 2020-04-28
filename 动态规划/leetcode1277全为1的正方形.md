## 题目
给你一个 m * n 的矩阵，矩阵中的元素不是 0 就是 1，请你统计并返回其中完全由 1 组成的 正方形 子矩阵的个数。
输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释： 
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones

## 思考
如果用暴力法去统计边长为1,2...n的个数，会超时。因为没有用到前面已经计算过的结果。而动态规划就是会利用已经计算过的结果，所以更优<br/>
如果用dp[i][j]表示以matrix[i][j]为`右下角`方块时所能组成的最大正方形边长，（之所以分析最大边长是因为边长为3的话其实就是表示以当前方块为右下角所能组成的正方形个数有3个，即边长为1的1个，边长为2的1个，边长为3的1个）。结果直接累加dp[i][j]就行。<br/>
有matrix[2][3]==1，分析matrix[2][2]、matrix[1][3]、matrix[1][2]各自为右下角方块时能组成的最大边长，取最小值然后加1.即dp[2][3] = min(dp[2][2],dp[1][3],dp[1][2])+1.这里之所以没有舍弃掉dp[1][2]是因为左上角方块确实可能组成更大的正方形,所以不能舍弃。<br/>
由于算法涉及到左边和上一行的元素，所以要注意边界，当i==0 or j==0时dp[i][j]要么为1要么为0。

## code
```Python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m = len(matrix)
        n = len(matrix[0])
        dp = [[0]*n for _ in range(m)]
        ans = 0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:  #如果当前元素为0，则肯定不能以他为右下角元素组成正方形
                    continue
                elif i==0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1])+1
                ans+=dp[i][j]
        return ans
```
小tip：为什么只需考虑当前方块的上面、左边、左上的三个方块呢，因为循环的时候i和j是从小到大，其实相当于一步步累积结果了。

## 相似题
`221 最大正方形面积`：在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。<br/>
```Python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        m = len(matrix)
        n = len(matrix[0])
        dp = [[0]*n for _ in range(m)]
        maxLen = 0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == '0':  #这个题目的矩阵是str型
                    dp[i][j] = 0
                elif i == 0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = min(dp[i-1][j-1],dp[i-1][j],dp[i][j-1])+1
                maxLen = max(maxLen,dp[i][j])
        return maxLen**2
```
这种题都是用dp解的，不是用什么DFS。如果是求岛屿最大面积才是DFS，那种是只要连通就行，但是这种求正方形边长或者面积有额外隐性要求。