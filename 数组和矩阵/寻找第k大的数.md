要求使用快排。优化的话，还要使用二分的思想，每次把得到的index和k-1作比较。 而且因为是找第k大，所以快排采用的逆序。

## code
```java
import java.util.*;

public class Finder {
    public int findKth(int[] a, int n, int K) {
        // write code here
        return findK(a, 0, n - 1, K);
    }
    public int quickSort(int[] arr, int left, int right){
        int pivot = arr[left];
        while(left < right){
            while(left < right && arr[right] <= pivot){
                right--;
            }
            arr[left] = arr[right];
            while(left < right && arr[left] >= pivot){
                left++;
            }
            arr[right] = arr[left];
        }
        arr[left] = pivot;
        return left;
    }
    public int findK(int[] arr, int left, int right, int k){
        if(left <= right){
            int index = quickSort(arr, left, right);
            if(index == k - 1){
                return arr[index];
            }else if(index < k - 1){
                return findK(arr, index + 1, right, k);
            }else{
                return findK(arr, left, index - 1, k);
            }
        }
        return -1;
    }
}
```