``` java
class Solution {
    public boolean isPalindrome(int x) {
        //String a = new String(x + "");
        String a = String.valueOf(x);
        String b = new StringBuilder().append(a).reverse().toString();
        return a.equals(b);
    }
}

// 推荐
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0)	// 排除负数，带符号肯定不对称
            return false;
        int[] a = new int[20];
        int ind = 0;
        while (x != 0) {
            a[ind++] = x % 10;
            x = x / 10;
        }
        int l = 0, r = ind - 1;
        while (l < r) {
            if (a[l++] != a[r--])
                return false;
        }
        return true;
    }
}
```

方法一：直接将数字当成字符串处理，然后判断是否为回文字符串。注意String的初始化，如果用new String()，参数只能为String；如果用**静态方法String.valueOf()**，则可以是其他类型。

方法二：排除掉负数后，按从低位到高位的顺序获取数字并存入数组，然后用双指针遍历。

方法三：参考https://leetcode.com/articles/palindrome-number/，比较神奇。