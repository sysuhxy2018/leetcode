### 代码

``` java
import java.util.*;

// 时间复杂度O(n)，空间复杂度O(1)，in-place
class Solution {
    public void rotate(int[] nums, int k) {
        if (k <= 0 || nums.length == 0)
            return;
        int n = nums.length;
        k = k % n;
        reverse(nums, 0, n - k - 1);
        reverse(nums, n - k, n - 1);
        reverse(nums, 0, n - 1);
    }
    
    private void reverse(int[] a, int l, int h) {
        while (l < h) {
            swap(a, l, h);
            l++; h--;
        }
    }
    
    private void swap(int[] a, int l, int h) {
        int tmp = a[l];
        a[l] = a[h];
        a[h] = tmp;
    }
}
```



### 思路

和剑指的左旋字符串有异曲同工之妙，不同之处是这里是右旋数组。详细的分析参考剑指，这里就不多写了。

* 旋转区间[0, n - k  - 1]，即前n - k个 。（如果左旋，则是前k个）
* 旋转区间[n - k,  n - 1]，即后k个。（如果左旋，则是后 n - k 个）
* 旋转整个数组区间[0, n - 1]。



附上链接：[GitHub](https://github.com/sysuhxy2018/-offer/blob/master/%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.md)，[牛客网](https://www.nowcoder.com/practice/12d959b108cb42b1ab72cef4d36af5ec?tpId=13&tqId=11196&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)。