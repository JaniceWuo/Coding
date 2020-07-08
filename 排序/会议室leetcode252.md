## 题目
给定一个会议时间安排的数组，每个会议时间都会包括开始和结束的时间 [[s1,e1],[s2,e2],...] (si < ei)，请你判断一个人是否能够参加这里面的全部会议。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/meeting-rooms
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路
通过排序，按会议开始时间升序排序  然后遍历  只要下一个会议的开始时间晚于前一个会议的结束时间就行  但是必须要排序才能这样比较


## code
```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return a[0]-b[0];
            }
        });
        for(int i = 1;i<intervals.length;i++){
            if(intervals[i][0]<intervals[i-1][1]) return false;
        }
        return true;
    }
}
```