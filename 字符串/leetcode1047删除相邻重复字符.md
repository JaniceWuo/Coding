## 题目描述
给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。在 S 上反复执行重复项删除操作，直到无法继续删除。在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string

## 思考
每次删除相邻重复项后，剩下的字符可能出现新的重复相邻项，如abba。这样的话采取原始遍历法就要遍历好几次。<br/>
复杂度低的巧妙做法是利用栈，当遍历到的数和栈底数相同时，把栈底的数弹出。

## 代码
```Python
class Solution:
    def removeDuplicates(self, S: str) -> str:
        stack = []
        for s in S:
            if stack and s==stack[-1]:
                stack.pop()
            else:
                stack.append(s)
        return ''.join(stack)
```
时间复杂度O(N)