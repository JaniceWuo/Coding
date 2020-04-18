## 题目
给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-area-of-island

## 思考
用DFS

## code
法1：循环调用dfs
```Python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m,n = len(grid),len(grid[0])
        dire = [[0,-1],[0,1],[-1,0],[1,0]]
        def dfs(i,j):
            if i < 0 or j < 0 or i == m or j == n or grid[i][j]!=1:
                return 0 
            grid[i][j] = 0
            tmp = 1
            for d in dire:
                n_i,n_j =i+d[0], j+d[1]
                if 0<=n_i<m and 0<=n_j<n and grid[n_i][n_j]:
                    tmp += dfs(n_i, n_j)
            return tmp
        resul = 0
        for i in range(m):
            for j in range(n):
                resul = max(resul,dfs(i,j))
        return resul
```
法2:利用stack
```Python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m,n = len(grid),len(grid[0])
        dire = [[0,-1],[0,1],[-1,0],[1,0]]
        stack = []
        ans = 0
        for i in range(m):
            for j in range(n):
                if i<0 or j<0 or i==m or j == n or grid[i][j]!=1:
                    continue
                stack.append([i,j])
                tmp = 1
                grid[i][j] = 0
                while stack:
                    node = stack.pop()
                    i_,j_ = node[0],node[1]
                    for d in dire:
                        n_i,n_j = i_+d[0],j_+d[1]
                        if 0<=n_i<m and 0<=n_j<n and grid[n_i][n_j]:
                            stack.append([n_i,n_j])
                            tmp+=1
                            grid[n_i][n_j]=0
                ans = max(ans,tmp)
        return ans
```
法2时间复杂度和空间复杂度都要低些