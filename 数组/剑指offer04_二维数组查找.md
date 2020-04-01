# 二维数组中的查找
## 题目描述
在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof

## 思考
这道题的数组有着从左到右递增，从上到下递增的特性，要利用该特性降低时间复杂度。令flag=matrix[n-1][0]，flag为一列最大一行最小，如果target小于flag，则去掉该行只需要往上找；若target大于flag，则去掉该列往右找。
## Code
```Python
class Solution:
    def findNumberIn2DArray(self, matrix: List[List[int]], target: int) -> bool:
        if matrix == [] or matrix == [[]]:
            return False
        n = len(matrix)
        m = len(matrix[0]) 
        i = n-1
        j = 0
        while i >=0 and j<=m-1:
            flag = matrix[i][j]
            if flag>target:
                i-=1
            elif flag < target:
                j+=1
            else:
                return True 
        return False
```
