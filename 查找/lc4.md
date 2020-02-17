``` java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int l = nums1.length + nums2.length;
        if (l % 2 == 1)
            return findKth(nums1, 0, nums2, 0, (l + 1) / 2);
        else
            return (findKth(nums1, 0, nums2, 0, l / 2) + findKth(nums1, 0, nums2, 0, l / 2 + 1)) * 0.5;
    }
    
    private int findKth(int[] nums1, int l1, int[] nums2, int l2, int k) {
        // 确保nums1总是比较短的一方
        if (nums1.length - l1 > nums2.length - l2)
            return findKth(nums2, l2, nums1, l1, k);
        // 边界情况
        if (nums1.length - l1 == 0)
            return nums2[l2 + k - 1];
        if (k == 1)
            return Math.min(nums1[l1], nums2[l2]);
        // 一般情况
        int k1 = Math.min(k / 2, nums1.length - l1), k2 = k - k1;
        if (nums1[l1 + k1 - 1] < nums2[l2 + k2 - 1])
            return findKth(nums1, l1 + k1, nums2, l2, k2);
        else if (nums1[l1 + k1 - 1] > nums2[l2 + k2 - 1])
            return findKth(nums1, l1, nums2, l2 + k2, k1);
        else
            return nums1[l1 + k1 - 1];
    }
}
```

findKth返回两个数组（合并后）的第k个数，注意这里k指的不是下标而是**第几个**，**从1开始**。参数l1和l2表示剩余数组部分起始的位置。

findKth核心还是二分递归，每次都会砍掉剩余数组的前半部分。这也是为什么参数只有起始位置，因为终止位置始终保持在nums.length - 1。递归的终止情况考虑两个，nums1（较短的一方）剩余长度为0，以及k == 1。一般情况中首先将k拆成k1和k2两部分，然后比较nums1中剩余部分的第k1个和nums2中剩余部分的第k2个，哪个小，那一方就可以被砍掉一部分；如果都相等，直接返回其中一个作为结果。

要求中位数，首先看总长度l的奇偶，如果是奇，即求第(l + 1) / 2个；如果是偶，则求第l / 2和l / 2 + 1的平均数。再次强调，这里第几个不是指下标。

