``` java
class Solution {
    public int reverse(int x) {
        int sign = x >= 0 ? 1 : -1;
        long y = Long.parseLong(new StringBuilder().append(Math.abs((long)x)).reverse().toString()) * sign;
        return y > Integer.MAX_VALUE || y < Integer.MIN_VALUE ? 0 : (int)y;
    }
}

// 推荐
class Solution {
    public int reverse(int x) {
        int sign = x >= 0 ? 1 : -1;	// 符号
        long sum = 0, num = Math.abs((long)x);	// 翻转是用绝对值翻转
        // 不管num是正还是负，都是 == 0的时候终止
        while (num != 0) {
            sum = sum * 10 + num % 10;
            num /= 10;
        }
        sum *= sign;
        return sum > Integer.MAX_VALUE || sum < Integer.MIN_VALUE ? 0 : (int)sum;
    }
}
```

第一种方法就是非常暴力地，long转字符串，字符串转long来处理。注意两处细（坑）节（点）：

* StringBuilder初始化**只能用字符串**，而不能用其他类型。利用append方法可以添加字符串或其他类型。所以最好的方法是初始化只写**new StringBuilder()**，后面再append就行了。
* 涉及到溢出问题，所以结果 y 必须要用long。那么为什么取绝对值时 x 也要long呢？这是因为Math.abs方法在Integer.MIN_VALUE时得到的结果还是Integer.MIN_VALUE，还是负的。原因很简单，如果取2147483648明显就溢出了。所以为了避免溢出，x也要转为long。https://www.cnblogs.com/silence-x/p/10543388.html

第二种是常规方法，就是不断 / 10，然后取低位数字去累加，从而相当于完成翻转。

------

补充：负数的 / 和 %。

如-18 / 10 = -1，-18 % 10 = -8。所以其实和正数的很类似，只不过**保留了符号**。