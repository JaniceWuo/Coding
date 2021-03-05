## 题目
将给出的链表中的节点每k 个一组翻转，返回翻转后的链表
如果链表中的节点数不是k 的倍数，将最后剩下的节点保持原样
你不能更改节点中的值，只能更改节点本身。
要求空间复杂度 O(1)。

## 思考
每k个一组翻转，其实就可以用递归。 单独写一个k个节点翻转的函数。

## code
```java
public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode reverseKGroup (ListNode head, int k) {
        // write code here
        if(head == null || head.next == null) return head;
        ListNode cur = head;
        for(int i = 0; i < k; i++){
            if(cur == null) return head;
            cur = cur.next;
        }
        ListNode newHead = reverse(head,cur);
        head.next = reverseKGroup(cur,k); //head其实一直在往后移
        return newHead;
    }
    
    private ListNode reverse(ListNode head, ListNode tail){
        ListNode nextNode = null;
        ListNode pre = null;
        while(head!=tail){
            nextNode = head.next;
            head.next = pre;
            pre = head;
            head = nextNode;
        }
        return pre;
    }
}
```