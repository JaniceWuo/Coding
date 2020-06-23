## 题目
给定一棵二叉搜索树，请找出其中第k大的节点。

## 思考
二叉搜索树的特点是左子树的值比根节点小，右子树的值比根节点大。所以第k大的节点为【中序遍历】结果倒序的第k个。中序遍历可以直接用递归形式,想要获得倒序结果，通过【右 根 左】。

## code
1.使用递归
```java
class Solution {
    int res,k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;   //this.k是实例变量k  相当于把形参k赋值给全局变量k
        dfs(root);
        return res;
    }
    void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.right);  //只要右子树存在 就会一直遍历下去  直到找到最右（即最大值）的节点
        k--;
        if(k == 0){
            res = root.val;
            return;
        } 
        dfs(root.left);
    }
}
```
2.使用迭代 用栈保存节点  先遍历右子树 然后根节点 再左子树
```java
class Solution {
    int count = 0;
    public int kthLargest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curNode = root;
        while(curNode!=null || !stack.isEmpty()){
            if(curNode!=null){
                stack.push(curNode);
                curNode = curNode.right;
            }else{
                curNode = stack.pop();
                count++;
                if(count == k) return curNode.val;
                curNode = curNode.left;
            }
        }
        return 0;
    }
}
```
第二种方法效率不如递归。