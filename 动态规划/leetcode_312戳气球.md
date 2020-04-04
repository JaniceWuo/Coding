## 题目描述
戳气球，戳一个气球i得到的硬币为nums[left] * nums[right] * nums[i]，可以假设nums[-1]=nums[n]=1，求能得到的最大硬币数。<br/>
题目链接:https://leetcode-cn.com/problems/burst-balloons/

## 思考
典型动态规划题，但是如果正向思考，每戳一个气球，气球两边的球就会靠拢，下标会变。只能逆向思考，从戳的最后一个球出发，假设为k，则k把[i,j]区间划分为[i,k],[k,j]。

## 代码
```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        int n = nums.size();
        int dp[n][n];
        for (int i = 0;i < n;i++){
            for(int j = 0;j < n;j++)
                dp[i][j] = 0;
        }
        for(int step = 2;step < n;step++){  //区间范围，从2开始，最大为n-1
            for(int i = 0;i < n-step;i++){
                int j = i+step;
                for(int k=i+1;k<j;k++){
                    dp[i][j] = max(dp[i][j],dp[i][k]+dp[k][j]+nums[i]*nums[k]*nums[j]);
                }
            }
        }
        return dp[0][n-1];
    }
};
```