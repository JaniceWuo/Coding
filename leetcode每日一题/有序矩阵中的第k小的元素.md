## 题目
给定一个 n x n 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 k 小的元素。请注意，它是排序后的第 k 小元素，而不是第 k 个不同的元素。

## 思考
如果直接new一个数组来存储全部的元素，然后排序找第k小，这是暴力法。而且没有用到该矩阵的特性。所以有一种二分法。<br/>
每次求当前left和right的mid，然后从整个矩阵的左下角元素出发，找不大于mid的数的个数，统计出这个个数num，如果num>=k则表明要找的第k小的数肯定在mid的左边，所以另right=mid。在和mid比较时也要利用矩阵的性质，是要j++还是i--。

## code
```java
class Solution {
    int len;
    int[][] matrix;
    int k;
    public int kthSmallest(int[][] matrix, int k) {
        this.len = matrix.length;
        this.matrix = matrix;
        this.k = k;
        int left = matrix[0][0];
        int right = matrix[len-1][len-1];
        while(left < right){
            int mid = left + ((right - left)>>1);   //每次都要根据left和right来计算mid  mid是改变的  这里运用了位运算比直接进行数学运算快的原理
            if(GetMinMidNum(mid)){
                right = mid;
            }else{
                left = mid + 1;
            }
        }
        return left;
    }

    //统计整个矩阵小于等于mid的数
    public boolean GetMinMidNum(int mid){
        int num = 0;  //记录的是比mid小于等于的个数
        int i = len - 1;
        int j = 0;
        while(i >= 0 && j <= len-1){
            if(matrix[i][j] <= mid){
                num += i+1;   //注意是加i+1
                j++;  //整个一列小于等于mid的个数都已经累计了  所以只能往右走了
            }else{
                i--;
            }
        }
        return num >= k;
    }
}
```

