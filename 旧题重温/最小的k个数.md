``` java
import java.util.*;

// 快速切分
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ans = new ArrayList<>();
        // 特殊情况，k > input.length
        if (k > input.length || k <= 0)
            return ans;
        int l = 0, r = input.length - 1;
        while (l < r) {
            int ind = partition(input, l, r);
            if (ind == k - 1)
                break;
            else if (ind < k - 1)
                l = ind + 1;
            else
                r = ind - 1;
        }
        for (int i = 0; i < k; i++)
            ans.add(input[i]);
        return ans;
    }
    
    /*
    // 快排模板
    private void qsort(int[] nums, int l, int r) {
        if (l < r) {
            int ind = partition(nums, l, r);
            qsort(nums, l, ind - 1);
            qsort(nums, ind + 1, r);
        }
    }
    */
    
    private int partition(int[] nums, int l, int r) {
        int tmp = nums[l];
        while (l < r) {
            while (l < r) {
                if (nums[r] < tmp)
                    break;
                else
                    r--;
            }
            nums[l] = nums[r];
            while (l < r) {
                if (nums[l] > tmp)
                    break;
                else
                    l++;
            }
            nums[r] = nums[l];
        }
        nums[l] = tmp;
        return l;
    }
}

// 最大堆
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>(){
            @Override
            public int compare(Integer a, Integer b) {
                return b - a;
            }
        });
        ArrayList<Integer> ans = new ArrayList<>();
        if (k > input.length || k <= 0)
            return ans;
        for (int i = 0; i < k; i++)
            pq.offer(input[i]);
        for (int i = k; i < input.length; i++) {
            if (input[i] < pq.peek()) {
                pq.poll();
                pq.offer(input[i]);
            }
        }
        while (!pq.isEmpty())
            ans.add(pq.poll());
        return ans;
    }
}

// 堆调整，最小堆
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ans = new ArrayList<>();
        int n = input.length;
        if (k <= 0 || k > n)
            return ans;
        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(input, i, n);
        for (int i = 0; i < k; i++) {
            ans.add(input[0]);
            swap(input, 0, n - 1 - i);
            heapify(input, 0, n - 1 - i);
        }
        return ans;
    }
    
    /*
    // 堆排模板
    public void heapsort(int[] nums) {
        int n = nums.length;
        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(nums, i, n);
        for (int i = n - 1; i > 0; i--) {
            swap(nums, 0, i);
            heapify(nums, 0, i);
        }
    }
    */
    
    /*
    // 最大堆（顺序）
    private void heapify(int[] nums, int i, int size) {
        int left = 2 * i + 1;
        if (left >= size)
            return;
        int right = 2 * i + 2;
        int maxInd = left;
        if (right < size && nums[right] > nums[left])
            maxInd = right;
        if (nums[maxInd] > nums[i]) {
            swap(nums, maxInd, i);
            heapify(nums, maxInd, size);
        }
    }
    */
    
    // 最小堆（倒序）
    private void heapify(int[] nums, int i, int size) {
        int left = 2 * i + 1;
        if (left >= size)
            return;
        int right = 2 * i + 2;
        int minInd = left;
        if (right < size && nums[right] < nums[left])
            minInd = right;
        if (nums[minInd] < nums[i]) {
            swap(nums, minInd, i);
            heapify(nums, minInd, size);
        }
    }
    
    private void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
}

```

这题可以练习快排 + 堆。属于top k问题，其实求最小的 k 个和第 k 小的数本质上都一样。求第 k 小顺带就求了最小的 k 个。

因为我们的目标是让 **ind == k - 1**，所以如果 ind > k - 1，就在左边找；< k - 1，就在右边找。

用堆做相对就比较简单了。可以用**最小堆（默认）**/最大堆。当然，这里也可以考察堆排的写法。如果是堆排，则用最小堆，这样我们直接取堆顶（nums[0]）就是剩余的最小值，然后把它交换到尾部再不断调整堆。用最小堆写出来的堆排自然也是从大到小的，倒序；如果顺序，用最大堆，稍微改一下heapify中结点的比较关系即可。

------

简单讲解一下堆排算法思路。清晰图解：https://www.cnblogs.com/chengxiao/p/6129630.html



