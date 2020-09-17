``` java
public class Solution {
    private TreeNode ans;
    private int cnt;	// 注意cnt必须是全局变量，不能是局部变量！
    
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        cnt = k;
        inorder(pRoot);
        return ans;
    }

    private void inorder(TreeNode root) {
        if (root == null)
            return;
        inorder(root.left);
        cnt--;
        if (cnt == 0)
            ans = root;
        inorder(root.right);
    }

}
```

