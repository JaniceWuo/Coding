## 题目
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
例如，给定三角形：
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/triangle

## 思想
如果自顶向下，用贪心算法是不行的，不能保证最后的结果是最小值。逆向思考，自底向上。假设最小路径在倒数第二行时经过的是6，则最后的最小值一定会加上（6+1=7）,则把6更新为7；同理假设经过的是5，则把5更新为6；7更新为10。然后往上走。最后返回第一行的值即为结果。

## code
```Python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        m = len(triangle)
        if m == 1:
            return triangle[0][0]
        for i in range(m-2,-1,-1):  #从倒数第二行开始
            n = len(triangle[i])  #第i行有多少个数
            for j in range(n):
                triangle[i][j] += min(triangle[i+1][j],triangle[i+1][j+1])
        return triangle[0][0]   
```