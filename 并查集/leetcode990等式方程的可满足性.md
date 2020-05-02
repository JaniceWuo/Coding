## 题目
给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 equations[i] 的长度为 4，并采用两种不同的形式之一："a==b" 或 "a!=b"。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 true，否则返回 false。 

例如<br/>
输入：["a==b","b!=a"]
输出：false

## 思考
使用并查集，当等式为"=="时，合并两个字母，如上面的例子则有b的王变为a，然后执行到"b!=a"，发现是不等于，然后检查两个字母是否在一个集合里面了，如果在一个集合里则返回False，否则继续遍历，这里a和b已经在一个集合里了但是遇到了不等于号，所以返回False。<br/>
（注意，要对式里面的等式做一个排序，'!'要在后面判断，否则会出错）例如["a==b","b!=c","c==a"]应该返回False，但是如果不对等式排序，则：b的王为a，b的王和c的王不等继续遍历，c的王变为a，跳出循环。最后输出的是True，错误。

## code
```Python
class UF:
    parent = {}
    def __init__(self):
        self.parent = {chr(ord('a')+x):chr(ord('a')+x) for x in range(26)} #parent = {a:a,b:b.....}
        #要掌握这种写法

    def find(self,x):
        while x!=self.parent[x]:
            x = self.parent[x]
        return x

    def union(self,p,q):
        x = self.find(p)
        y = self.find(q)
        if x!=y:
            self.parent[y] = x
class Solution:
    def equationsPossible(self, equations: List[str]) -> bool:
        uf = UF()
        equations.sort(key = lambda x:x[1],reverse = True)
        for equa in equations:
            if equa[1]=='=':
                uf.union(equa[0],equa[3])
            else:
                if uf.find(equa[0]) == uf.find(equa[3]):
                    return False
        return True
```