就是找一个数组里的最小的k个数并输出。（这里k个数的输出并没有要求从小到大输出）

## 分析
有多种解法，最low的解法就是对整个数组排序：冒泡/选择/插入等。但是当数组规模过大的时候，排序算法要把n个数加载到内存中，效率很低，不适用于海量数据查找最小的k个数。
常用的就是堆排序，这里是找最小，那么就建大根堆。可以直接利用java中的优先队列构造大根堆，遍历后面的数的时候判断如果比堆顶的数大，就跳过，否则弹出堆顶，将当前数插入。
但是可能面试官不让用这种集合类，让手建大根堆考研数据结构功底：首先是从len/2的位置开始，然后逐步向上调整为大根堆。每一步调整的过程，其实都要向下筛选比较，找子节点是否有比它大的数。这里还要定义一个哨兵位置，arr[0]，用来保存父节点的值。
还有一种快排。O(n)，但是只能当可以修改原数组的时候可用。而且这里的快排不是之前我理解的那种，一开始pivotKey为nums[start]，这里是右边找到第一个比pivotKey小的数之后就立马交换，左边找到第一个大于pivotKey的数之后也立马交换。最后start下标的左边的数一定是比它小的，右边的数是比它大的。但是用这种快排输出的前k个最小的数也不是按从小到大输出的。

## code
利用优先队列
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        PriorityQueue<Integer> maxQueue = new PriorityQueue<>((o1,o2)->(o2-o1));
        ArrayList<Integer> res = new ArrayList<>();
        if(input == null || input.length < k || k == 0) return res;
        for(int i = 0; i < input.length;i++){
            if(maxQueue.size() < k){
                maxQueue.offer(input[i]);
            }else{
                if(maxQueue.peek() > input[i]){
                    maxQueue.poll(); //将堆顶弹出 也就是将最大的弹出
                    maxQueue.offer(input[i]);
                }
            }
        }
        res.addAll(maxQueue);
        return res;
    }
}
```
手动建大根堆
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k){
        ArrayList<Integer> list  = new ArrayList<>(  );
        if(input == null || input.length == 0 || k > input.length || k == 0) return list;
        int[] arr = new int[k+1];
        //第0个位置是空的  为了让下标和第几个元素是对应的  方便建堆  且可以空出一个哨兵位置
        for (int i = 1; i <= k; i++){
            arr[i] = input[i-1];
        }
        buildMaxHeap( arr,k );
        for (int i = k; i < input.length; i++){
            if(input[i] < arr[1]){
                arr[1] = input[i];
                AdjustDown( arr,1,k );
            }
        }
        for (int i = 1; i < arr.length; i++){
            list.add( arr[i] );
        }
        return list;

    }
    public void buildMaxHeap(int[] arr,int length){
        if(arr == null || arr.length == 0 || arr.length == 1){
            return;
        }
        //都是从length/2 开始向上调整的
        for (int i = length/2; i > 0; i--){
            AdjustDown( arr, i, arr.length - 1 );
        }
    }
    public void AdjustDown(int[] arr, int k, int length){
        arr[0] = arr[k];
        for (int i = 2 * k; i <= length; i *= 2){
            if(i < length  && arr[i] < arr[i+1]) i++;  //因为这一步涉及到i+1 所以i只能小于length
            if( arr[0] >= arr[i]) break;
            else {
                arr[k] = arr[i];  //将父节点赋予最大值
                k = i;  //k变成了子孩子的下标  为了接下来可能还要向下找做准备
            }
        }
        arr[k] = arr[0];  //子孩子赋予了最初父节点的值
    }
}
```
利用TreeSet的自动排序，最后一个元素就是当前最大的。  适合海量数据。
这种输出的结果是从小到大输出。  o(Nlogk)
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        if(input == null || input.length < k || k == 0) return res;
        TreeSet<Integer> kSet = new TreeSet<>(  );
        for (int i = 0; i < len; i++){
            if (kSet.size() < k){
                kSet.add( input[i] );
            }else {
                if(input[i] < kSet.last()){
                    kSet.remove( kSet.last() );
                    kSet.add( input[i] );
                }
            }
        }
        Iterator<Integer> iterator = kSet.iterator();
        while (iterator.hasNext()){
            list.add( iterator.next() );
        }
        return res;
    }
}
```

快排
```java
public class Main {
    public static void main(String[] args) {
        int nums[] = new int[]{3,4,5,1,2,6,7};
        ArrayList<Integer> list = GetLeastNumbers_Solution( nums, 4 );
        System.out.println(list); //[2,1,3,4]
    }
    public static ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k){
        ArrayList<Integer> list  = new ArrayList<>(  );
        int len = input.length;
        if (input == null || len == 0 || k > len || k == 0) return list;
        int start = 0;
        int end = len - 1;
        int index = partion( input, start, end );
        while (index != k-1){
        	//如果index大于了k-1， 说明k个最小数字一定在index的左边 所以往左划分
            if (index > k-1){
                end = index - 1;
                index = partion( input, start, end );
            }else {
                start = index + 1;
                index = partion( input, start, end );
            }
        }
        for (int i = 0; i < k; i++){
            list.add( input[i] );
        }
        return list;

    }

    public static int partion(int[] nums, int start, int end){
        int pivotKey = nums[start];
        while (start < end){
            while (start < end && pivotKey <= nums[end]){
                end--;
            }
            nums[start] = nums[end];
            while (start < end && pivotKey >= nums[start]){
                start++;
            }
            nums[end] = nums[start];
        }
        nums[start] = pivotKey;
        return start;
    }
}
```