## 题目
把字符串 s 看作是“abcdefghijklmnopqrstuvwxyz”的无限环绕字符串，所以 s 看起来是这样的："...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....". 
现在我们有了另一个字符串 p 。你需要的是找出 s 中有多少个唯一的 p 的非空子串，尤其是当你的输入是字符串 p ，你需要输出字符串 s 中 p 的不同的非空子串的数目。 
注意: p 仅由小写的英文字母组成，p 的大小可能超过 10000。<br/>
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-substrings-in-wraparound-string<br/>
比如  输入: "zab" 输出: 6 解释: 在字符串 S 中有六个子串“z”、“a”、“b”、“za”、“ab”、“zab”。

## 思考
如果是以step为1，i=0开始遍历找子串，太费时了，而且还要去重。<br/>
巧妙办法：记录以当前字母为结束的最大子串数的个数。比如"zaxyza"，起初dp['a']=2，表示'a','za';但是dp['a']最后等于4，表示的是'a','za','yza','xyza'，选最大值4，其实就已经包含了等于2时的子串，避免重复。一般定义dp为int型，所以dp['a'-'a']=dp[0]=4。

## 代码
```c++
class Solution {
public:
    int findSubstringInWraproundString(string p) {
        int N = p.size();
        if(N==1) return 1;
        if(N==0) return 0;
        vector<int> dp(26,0);  //26个字母 
        int count = 1;
        dp[p[0]-'a'] = 1;  //因为想i从1开始遍历，所以要先存一下p[0]的情况
        for(int i = 1;i < N;i++){
            if(p[i]-p[i-1]==1 || p[i]-p[i-1]==-25) count++;  //‘za’也是连续的
            else count = 1;
            dp[p[i]-'a'] = max(dp[p[i]-'a'],count);
            // cout<<dp[p[i]-'a']<<endl;
        }
        return accumulate(dp.begin(),dp.end(),0);  //起始值设为0
    }
};
```
