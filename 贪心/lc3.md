``` java
class Solution {
    
    public int lengthOfLongestSubstring(String s) {
        int[] cs = new int[128];
        Arrays.fill(cs, -1);
        int ml = 0, start = 0;
        for (int i = 0; i < s.length(); i++) {
            if (cs[s.charAt(i)] < start)
                cs[s.charAt(i)] = i;
            else {
                ml = Math.max(ml, i - start);
                start = cs[s.charAt(i)] + 1;
                cs[s.charAt(i)] = i;
            }
        }
        ml = Math.max(ml, s.length() - start);
        return ml;
    }
}
```

