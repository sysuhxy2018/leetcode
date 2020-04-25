``` java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> rec = new HashSet<>();
        for (int num : nums)
            rec.add(num);
        int ans = 0;
        for (int num : nums) {
            // 加if的目的是为了避免重复遍历相关元素，提高效率
            // 相当于只考虑从所有连续序列的第一个元素开始检查
            if (!rec.contains(num - 1)) {
                int len = 0;
                while (rec.contains(num)) {
                    len++;
                    num++;
                }
                ans = Math.max(ans, len);
            }
        }
        return ans;
    }
}
```

