## 题目
合并两个有序链表
将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
示例：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

## code
```Python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        if l1.val <= l2.val:
            l1.next = self.mergeTwoLists(l1.next,l2)  #将l1.next指向l1.next和l2做merge的结果
            return l1
        else:
            l2.next = self.mergeTwoLists(l2.next,l1)
            return l2
```
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 }

class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l2 == null) return l1;
        else if(l1 == null) return l2;
        else{
            if(l1.val < l2.val){
                l1.next = mergeTwoLists(l1.next,l2);
                return l1;
            }else{
                l2.next = mergeTwoLists(l1,l2.next);
                return l2;
            }
        }
    }
}
```

非递归版本，就是遍历比较大小。最好是定义一个辅助头节点，更方便。
```java
public class Solution {
    /**
     * 
     * @param l1 ListNode类 
     * @param l2 ListNode类 
     * @return ListNode类
     */
    public ListNode mergeTwoLists (ListNode l1, ListNode l2) {
        // write code here
        if(l1==null) return l2;
        if(l2==null) return l1;
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while(l1!=null && l2!=null){
            if(l1.val <= l2.val){
                tail.next = l1;
                l1 = l1.next;
            }else{
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }
        tail.next = l1 == null ? l2 : l1;
        return head.next;
    }
}
```