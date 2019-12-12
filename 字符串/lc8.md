### 代码

``` java
class Solution {
    public int myAtoi(String str) {
        int n = str.length();
        int j = 0;
        while (j < n && str.charAt(j) == ' ')
            j++;
        long sum = 0;
        boolean sign = true;
        for (int i = j; i < n; i++) {
            if ((sign && sum > Integer.MAX_VALUE) ||
                (!sign && -sum < Integer.MIN_VALUE))
                break;
            char c = str.charAt(i);
            if (i == j && c == '-')
                sign = false;
            else if (i == j && c == '+')
                sign = true;
            else if (Character.isDigit(c))
                sum  = sum * 10 + (c - '0');
            else
                break;
        }
        if (!sign)
            sum = -sum;
        if (sum > Integer.MAX_VALUE)
            sum = Integer.MAX_VALUE;
        else if (sum < Integer.MIN_VALUE)
            sum = Integer.MIN_VALUE;
        return (int)sum;    
    }
}

// 简化版
class Solution {
    public int myAtoi(String str) {
        int n = str.length();
        int i = 0;
        while (i < n && str.charAt(i) == ' ')
            i++;
        
        long sum = 0;
        boolean sign = true;
        if (i < n && str.charAt(i) == '-') {
            sign = false;
            i++;
        }
        else if (i < n && str.charAt(i) == '+')
            i++;
        
        for (int j = i; j < n; j++) {
            char c = str.charAt(j);
            if (!Character.isDigit(c))
                break;
            sum = sum * 10 + c - '0';
            if (sign && sum > Integer.MAX_VALUE)
                return Integer.MAX_VALUE;
            else if (!sign && -sum < Integer.MIN_VALUE)
                return Integer.MIN_VALUE;
        }
        
        return sign ? (int)sum : (int)-sum;
    }
}
```



### 思路

和剑指的字符串转整数比较类似，但是还是有些许不同，这里就不放重复链接了。下面以简化版为例介绍一下思路。

* 首先题目明确要求我们忽略开头的连续空白字符(whitespace)。注意这里只有' '被视为空白字符，像'\n'和'\t'这样的特殊字符并不能忽略掉。也就是说我们不能使用字符串的trim方法，需要自己写。其实也挺简单的。
  * 用while循环跳过所有空格，终止条件为 j >= n 或者 str.charAt(j) != ' '。
* 由于可能会涉及到 int 的溢出，所以计算数值的 sum 类型设置为long而不是 int，sum初始化为0。注意这里sum相当于**绝对值**，是不包含符号的。此外维护一个布尔变量sign，用于表示符号，初始化为true，表示为正（非负）。
* 从上面得到的第一个有效位置 j 开始，先判断符号。因为 j 可能 >= n了，所以要判断 j 的下标有效性。
  * j < n && str.charAt(j) == '-'，说明是负数，则sign更新为false。j++
  * j < n && str.charAt(j) == '+'，说明是正数，sign不变，但是 j 还是要更新，因为符号位不能用于计算数值。
  * 其余情况下，说明没有符号位，则 j 保持不变，不需要移动更新。
* 然后就可以从新的 j 位置开始遍历计算数值了。
  * 遇到非数字的字符，直接break掉。
  * 遇到数字字符，则继续计算sum。计算方式如下：
    * sum更新为前一次sum * 10 + 当前位的数字，即sum = sum * 10 + (c - '0')。这里c - '0'的目的是将一个数字字符转换为对应的 int 数值。 
  * 每更新一次sum，需要检查是否已经溢出了，如果已经溢出就没有继续计算的必要了。
    * 如果是上溢，即sign为true且sum > Integer.MAX_VALUE，则直接返回最大int值Integer.MAX_VALUE
    * 如果是下溢，即sign为false且-sum < Integer.MIN_VALUE，则直接返回最小int值Integer.MIN_VALUE
* 最后根据sign返回对应的值，正（非负）数为sum，负数为-sum。注意返回值类型要求为int，所以需要将类型为long的sum/-sum做一个显式的**类型转换**。



### 总结

* 原版的存在是有价值的，体现在？
* 为什么要先计算sum再判断溢出？
* 有没有别的不用long的判断int溢出的好方法？
* 其他的一些坑点。