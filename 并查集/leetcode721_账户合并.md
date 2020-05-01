## 题目
给定一个列表 accounts，每个元素 accounts[i] 是一个字符串列表，其中第一个元素 accounts[i][0] 是 名称 (name)，其余元素是 emails 表示该帐户的邮箱地址。

现在，我们想合并这些帐户。如果两个帐户都有一些共同的邮件地址，则两个帐户必定属于同一个人。请注意，即使两个帐户具有相同的名称，它们也可能属于不同的人，因为人们可能具有相同的名称。一个人最初可以拥有任意数量的帐户，但其所有帐户都具有相同的名称。

合并帐户后，按以下格式返回帐户：每个帐户的第一个元素是名称，其余元素是按顺序排列的邮箱地址。accounts 本身可以以任意顺序返回。<br/>
例如:
Input: 
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]

##思路
其实思考一下发现跟`朋友圈`那题很像，比如"johnsmith@mail.com"和"john00@mail.com"是一对好朋友，"johnsmith@mail.com"和"john_newyork@mail.com"也是好朋友，那么他们三个是一个圈子里的好友（用并查集）。最后只需要再找到他们所属的那个账号就行，所以要先用email2name存一下。

## code
```Python
class UF:
    def __init__(self):
        self.parent = {}

    def find(self, x):
        self.parent.setdefault(x, x)
        while x != self.parent[x]:
            x = self.parent[x]
        return x 
    def union(self, p, q):
        x = self.find(p)
        y = self.find(q)
        if x!=y:
          self.parent[x] = y  

class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        uf = UF()
        res = collections.defaultdict(list)
        email2name = {}
        for account in accounts:
            for i in range(1,len(account)):
                email2name[account[i]] = account[0]
                if i != len(account)-1:
                    uf.union(account[i],account[i+1])   #执行合并
        for email in email2name:
            res[uf.find(email)].append(email) #将归属于同一个父亲节点的全部子节点集合起来
        ans = []
        for value in res.values():
            res1 = [email2name[value[0]]] + sorted(value)
            ans.append(res1)
        return ans
```