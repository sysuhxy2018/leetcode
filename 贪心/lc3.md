``` java
// sliding window
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int i = 0, j = 0;
        Set<Character> st = new HashSet<>();
        int ma = 0;
        while (i < n && j < n) {
            // 没有重复，说明[i, j]是有效的，可以更新ma
            if (!st.contains(s.charAt(j))) {
                ma = Math.max(ma, j - i + 1);
                st.add(s.charAt(j));
                j++;
            }
            // 重复，不断更新i使得[i, j)中不存在该字符
            else {
                st.remove(s.charAt(i));
                i++;
            }
        }
        return ma;
    }
}

// optimized
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int[] pos = new int[128];
        Arrays.fill(pos, -1);	// 防止和有效位置0混淆，统一初始为无效值-1
        int i = 0, ml = 0;
        for (int j = 0; j < n; j++) {
            if (pos[s.charAt(j)] >= i)	// 重复时，i直接跳到新的位置，让[i, j]始终有效
                i = pos[s.charAt(j)] + 1;
            ml = Math.max(ml, j - i + 1);
            pos[s.charAt(j)] = j;	// 记得更新最新出现的位置
        }
        return ml;
    }
}
```

这题实际是一个滑动窗口的问题。滑动窗口和双指针比较类似，不过一般双指针的移动方向是反向的，而滑动窗口是同向的。

这里我们选择[i, j]作为窗口，如果 s(j) 没有在set中出现过，说明没有重复，则我们可以让 i 不动，不断右移扩张 j，同时添加 s(j) 到 set里；否则，出现重复，这时**让 j 不动**，不断**右移 i** 从而缩小窗口直到 [i , j) 中不存在该字符，同时记得移除set里对应的s(i)。

如果仔细分析的话，会发现重复时，i 只要移动到上一次**s(j)出现位置的下一位**即可，所以我们必须记录每个字符出现的位置。同时考虑到只有ascii字符，所以可以用hash table而不需要map。那么这里会存在一个问题，我们直接让 i 移动到新的位置，但是中间这些本应该被remove的字符怎么办？换句话说，我们必须能够辨别要遍历的字符是否在新的有效[i, j]区间出现过。其实很简单，只要它最近一次**出现的位置 >= i**，则表明它在区间内，即重复。

那么根据上面的分析，我们可以**让 j 一直右移**，**重复时更新 i**，然后每次记录有效区间 [i, j] 的最大长度即可。