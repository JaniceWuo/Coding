## 题目
班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/friend-circles

## 思考
其实就是求连通子图的个数。

## code
方法一：使用并查集<br/>
起初每个节点的王都是自己，初始化为-1。比如一开始是结点0，parent[0]=-1，则0的王是他自己0。然后数组里面M[0][1]==1，表示节点0和节点1相遇，1的王是1，不相同要打架，假设0赢了，则parent[1]=0,接着又有M[0][2]==1，即0遇到了节点2，则0和2PK，假设0赢了，则parent[2]=0。此时只有parent[0]为-1，则输出1，表示就一个连通图。
```Python
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        m = len(M)
        parent = [-1 for _ in range(m)]
        def find_parent(parent,i):
            if parent[i]==-1:
                return i
            return find_parent(parent,parent[i])
        def union(i,j):
            i_parent = find_parent(parent,i)
            j_parent = find_parent(parent,j)
            if i_parent != j_parent:
                parent[j_parent] = i_parent

        def helper(Matrix):
            for i in range(m):
                for j in range(i,m):
                    if Matrix[i][j] == 1 and i != j:
                        union(i,j)
            count = 0
            # print(parent)
            for i in range(m):
                if parent[i]==-1:
                    count+=1
            return count
        return helper(M)
```
方法2：DFS
```Python
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        m = len(M) 
        def dfs(i,visited):
            for j in range(m):
                if M[i][j] == 1 and visited[j]==-1:
                    visited[j] = 1
                    dfs(j,visited)
        count = 0
        visited = [-1]*m
        for i in range(m):
            if visited[i] == -1:
                dfs(i,visited)
                count+=1
        return count
```