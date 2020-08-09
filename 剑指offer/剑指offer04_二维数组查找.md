# 二维数组中的查找
## 题目描述
在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 思考
这道题的数组有着从左到右递增，从上到下递增的特性，要利用该特性降低时间复杂度。

## Code
```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        int m = array.length;
        int n = array[0].length;
        int i = m -1;
        int j = 0;
        while(i >= 0 && j <= n-1){  //注意i和j都有边界
            if(array[i][j] == target){
                return true;
            }
            if(array[i][j] < target){
                j++;
            }else{
                i--;
            }
        }
        return false;
    }
}
```
