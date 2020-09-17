``` java
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        return gen(pRootOfTree)[0];
    }
    
    // 递归返回链表的head和tail
    private TreeNode[] gen(TreeNode root) {
        if (root == null)
            return new TreeNode[]{null, null};
        TreeNode[] left = gen(root.left);
        TreeNode[] right = gen(root.right);
        TreeNode[] res = new TreeNode[2];
        // 为了保证中序，root还是放到中间
        if (left[1] != null) {
            left[1].right = root;
            root.left = left[1];
            res[0] = left[0];
        }
        else
            res[0] = root;
        if (right[0] != null) {
            root.right = right[0];
            right[0].left = root;
            res[1] = right[1];
        }
        else
            res[1] = root;
        return res;
    }
}
```

