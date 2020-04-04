## 题目描述
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。
说明：
拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。<br/>
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-break

## 思想
dp 判断前dp[i-当前遍历的单词数]是否为1以及s中是否有当前单词数  还要用到c++的compare函数 只有完全匹配时为0。

## 代码
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        if(s.empty()||wordDict.empty()) return false;
        int n = s.size();
        vector<int> dp(n+1,0);  //vector不提倡bool  dp[i]为前i个单词是否能够用wordDict中单词构成
        dp[0] = 1; 
        for(int i =1;i <= n;i++){
            for(auto w:wordDict){
                int wordSize = w.size();
                if(i-wordSize>=0){
                    int mat = s.compare(i-wordSize,wordSize,w);  //compare(i,s,s2)是从第i个字符开始包含s个字符和s2比较
                    if(!mat && dp[i-wordSize]) dp[i] = 1; 
                }
            }
        }
        return dp[n];
    }
};

```
