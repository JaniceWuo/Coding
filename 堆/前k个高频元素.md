给定一个非空的整数数组，返回其中出现频率前K高的元素。
比如[1,1,1,2,2,3] k = 2.则输出[1,2]。

## 思路
用最小堆。 但是一开始也要用hashmap存储数字和出现次数。 用优先队列模拟堆，泛型为int[]，按照出现次数从低到高排序。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0) + 1);
        }
        PriorityQueue<int[]> queue = new PriorityQueue<>(
            new Comparator<int[]>(){
                public int compare(int[] m, int[] n){
                    return m[1] - n[1];
                }
            }
        );
        for(Map.Entry<Integer,Integer> entry:map.entrySet()){
            int num = entry.getKey();
            int value = entry.getValue();
            if(queue.size() == k){
                if(queue.peek()[1] < value){
                    queue.poll();
                    queue.offer(new int[]{num,value});
                }
            }else{
                queue.offer(new int[]{num,value});
            }
        }
        int[] res = new int[k];
        for(int i = k-1; i >= 0; i--){
            res[i] = queue.poll()[0];
        }
        return res;
    }
}
```
时间复杂度，O(Nlogk)。