## 题目
就是对一个无序链表排序，4->2->1->3排序为1->2->3->4。

## 思路
使用归并排序，把链表从中间断开分成两段，然后分别对两段再进行划分直到只剩一个结点。这里找链表中点的技巧是使用`快慢指针`，当快指针到达链表末尾时，若链表长度为奇数，则慢指针到达中间点，若为偶数，则慢指针到达中间点的左边一个。<br/>
当慢指针到达中点后，要先用另一个指针second指向slow.next部分，即作为第二条链表的头，然后令slow.next=None，这里一定要变为None,才是真正断成两段链表。然后使用递归不断的将两条链表进行拆分。最后使用merge,merge函数其实就是把两条有序链表进行合并的非递归算法，要注意的是要定义head = cur = ListNode(-1),cur会不断移动最后返回head.next。

## code
```Python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:   #之所以要判断head.next是否为空是因为若只有一个结点时也不需要继续划分链表了
            return head
        slow,fast = head,head   #利用快慢指针寻找链表中点
        while fast.next and fast.next.next:
            fast = fast.next.next
            slow = slow.next
        secondHead = slow.next
        slow.next = None  #进行断链
        right_node = self.sortList(secondHead)
        left_node = self.sortList(head)
        return self.merge(left_node,right_node)

    def merge(self,leftList,rightList):
        head = cur = ListNode(-1)
        while leftList and rightList:
            if leftList.val <= rightList.val:
                cur.next = leftList
                leftList = leftList.next
            else:
                cur.next = rightList
                rightList = rightList.next
            cur = cur.next  
        #这里cur肯定要往后移啊，不然cur一直在原地不动的话执行cur.next的时候其实就一直在对原来那个节点的next进行操作

        cur.next = leftList if leftList else rightList
        return head.next
```

其实还有自底向上的归并排序，时间复杂度更低，但是目前还没掌握。