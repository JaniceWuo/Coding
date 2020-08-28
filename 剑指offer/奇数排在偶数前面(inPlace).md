输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

这题要求原地置换。如果可以开辟另一个数组的话，就很简单。
原地置换那就需要两个指针i和j，i找第一个偶数，j从i+1位置开始找第一个奇数，之后把[i,j-1]区间的数往后挪一位。

## code
```java
import java.util.*;
public class Solution {
    public void reOrderArray(int [] array) {
        int i = 0;
        int len = array.length;
        if(len <= 1) return;
        while( i < len){
            if(array[i] % 2 == 1){
                i++;
            }else{
                int j = i+1;
                if(j >= len) return;  //这里要判断一下
                while(array[j] % 2 == 0){
                    if(j == len - 1) return;
                    j++;
                }
                int temp = array[j];
                while(j > i){
                    array[j] = array[j-1];  //往后挪一位 也就是 复制
                    j--; 
                }
                array[i] = temp;
            }
        }
    }
}
```
