``` java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        // 注意判定特殊情况，[]
        if (sequence.length == 0)
            return false;
        return judge(sequence, 0, sequence.length - 1);
    }
    
    private boolean judge(int[] seq, int i, int j) {
        if (i >= j)
            return true;
        int root = seq[j];
        // 利用二叉树的大小关系，可以分割出左右子树
        int k = i;
        while (k < j && seq[k] < root)
            k++;
        int mid = k;
        while (k < j && seq[k] > root)
            k++;
        if (k == j)
            return judge(seq, i, mid - 1) && judge(seq, mid, j - 1);
        else
            return false;
    }
}
```

