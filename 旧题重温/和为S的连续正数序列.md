``` java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        // 注意窗口大小至少为2
        int start = 1, end = 2, total = 3;
        // 关键是total始终要包含[start, end]里的和，这样 == sum时可以直接得到序列[start, end]
        while (end < sum) {
            if (total < sum) {
                end++;
                if (end < sum)
                    total += end;
            }
            else if (total > sum) {
                total -= start;
                start++;
            }
            else {
                ArrayList<Integer> tmp = new ArrayList<>();
                for (int i = start; i <= end; i++)
                    tmp.add(i);
                ans.add(tmp);
                // start和end都要更新（当然total也要随之更新）
                total -= start;
                start++;
                end++;
                if (end < sum)
                    total += end;
            }
        }
        return ans;
    }
}
```

