链接：https://www.nowcoder.com/questionTerminal/a632ec91a4524773b8af8694a51109e7
来源：牛客网

给出一个链表，每 k 个节点一组进行翻转，并返回翻转后的链表。k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么将最后剩余节点保持原有顺序。

说明：
1. 你需要自行定义链表结构，将输入的数据保存到你的链表中；
2. 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换；
3. 你的算法只能使用常数的额外空间。

## 分析
难点1：自己定义链表结构。 考察 对数据结构的掌握程度。
难点2：要真正的进行节点交换，也就说要掌握pre next这些的运用以及指针的指向。
难点3：当第一部分的k个节点翻转后， 怎么连接到后面的链表。这里要用到一个指针指向当前最后一个节点，然后这个指针的next为下一段翻转的结果，也就是前一段的最后一个节点的next是下一段的头。这样就连上了。 而且用到了递归。

## code
```java
import java.util.Scanner;

class ListNode{
    public int val;
    public ListNode next = null;
    public ListNode(int val) {
        this.val = val;
    }
}
public class Main {
    static int allLen = 0;
    public static void main(String[] args) {
        Scanner sc = new Scanner( System.in );
        String line = sc.nextLine();
        String[] nums = line.trim().split( " " );
        allLen = nums.length;
        ListNode head = new ListNode( 0 );  //头结点  是不动的
        ListNode cur = head;  //cur起初等于head  后面是要移动的
        //建立带头结点的链表
        for (String num : nums) {
            int curNum = Integer.valueOf( num );
            ListNode newNode = new ListNode( curNum );
            cur.next = newNode;  //其实也就是head.next指向了newNode
            cur = cur.next;
        }
        int k = sc.nextInt();
        head = head.next;
        ListNode node = reverseGroup( head,k );
        int num = 1;
        while (num < allLen){
            System.out.print(node.val + " ");
            node = node.next;
            num++;
        }
        System.out.print(node.val);
    }

    //采用递归
    public static ListNode reverseGroup(ListNode head, int k){
        if(head == null || head.next == null || k==1 || k>allLen){
            return head;
        }
        ListNode curNode = head;
        for (int i = 0; i < k-1;i++){
            curNode = curNode.next;
            if(curNode == null){
                return head;  //如果还没到k个就已经为空了 说明这部分不用翻转
            }
        }
        ListNode next = curNode.next;
        reverse( head, curNode );  //将头和尾指针中间的值翻转
        head.next = reverseGroup( next,k );  //这句非常重要！ 是将之前翻转的结果和后面的结果连接起来  这时的head已经变成了翻转后的尾指针
        return curNode;  //虽然一开始是curNode是一段的尾指针，但是经过reverse后，变成头了。其实它本身没动，只是内部节点的方向变了。
    }

    public static ListNode reverse(ListNode head, ListNode tail){
        if(head == null || head.next == null) return head;
        ListNode pre = null;
        ListNode next = null;
        while (pre!=tail){
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
}
```