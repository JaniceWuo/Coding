## 题目
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
示例： [0,1,0,2,1,0,1,3,2,1,2,1]  输出:6

## 思考
此题有多种解法，我们来一步步优化时间复杂度。
第一种解法：遍历每个柱子求它能接的雨水。要找当前遍历的柱子的左右两边柱子最高值，取最矮的，减去当前遍历的柱子的高度，就是它能接住的雨水量。时间复杂度O(n^2),空间复杂度为O(1)。
第二种解法：动态规划。为了避免每次遍历一个柱子，都要去左右两边去找最高的柱子造成双重循环，利用好递推关系式，max_left[i]表示当前柱子i的左边全部柱子的最高值，有max_left[i] = Math.max(max_left[i-1],height[i-1])。这样时间复杂度为O(n),空间复杂度为O(n)。
动态规划还可以优化空间复杂度，还有栈的解法，这里都不写了。
## code
法一：
```java
class Solution {
    public int trap(int[] height) {
        int res = 0;
        for(int i = 1;i<height.length - 1;i++){
            int max_left = 0;
            for(int j = i-1;j>=0;j--){
                if(height[j]>max_left) max_left = height[j];
            }
            int max_right = 0;
            for(int j = i+1;j<height.length;j++){
                if(height[j]>max_right) max_right = height[j];
            }
            int tmp = Math.min(max_right,max_left);
            res += tmp>height[i]?tmp-height[i]:0;
        }
        return res;
    }
}
```
法二：
```java
class Solution {
    public int trap(int[] height) {
        int res = 0;
        int[] max_left = new int[height.length];
        int[] max_right = new int[height.length];
        for(int i = 1;i<height.length-1;i++){
            max_left[i] = Math.max(max_left[i-1],height[i-1]);
        }
        for(int i = height.length - 2;i>=1;i--){
            max_right[i] = Math.max(max_right[i+1],height[i+1]);
        }
        for(int i = 1;i<height.length-1;i++){
            int tmp = Math.min(max_right[i],max_left[i]);
            res += tmp-height[i]>0?tmp-height[i]:0;   
        }
        return res;
    }
}
```