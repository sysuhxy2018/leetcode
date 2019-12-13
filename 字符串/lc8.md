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

* 第一版代码还是有些可取之处的。比如它不需要单独考虑符号位，而是将所有情况合并在一起写，有种“大杂烩”的感觉。这其实也是一种思路，就是把我们所能确认到的情况都列出来，然后进行相应的操作，剩下的就交给else然后统一处理。需要注意的是，这些情况间必须完全独立，即不能出现包含/被包含的关系。

* 在简化版里，先计算sum再判断溢出。因为如果是先判断的话，sum可能在最后一轮更新后才溢出，也就是还需要在for循环外面再判断一次是否溢出。关于判断语句应该写在更新语句前还是后的问题，可以参考以下准则：

  * 其实两种写法效果基本类似。区别在于写在前面，无法判断最后一次执行的结果；写在后面，无法判断初始化（即第一次执行前）的结果。
  * 一般的话，我们都会写在**前面**，因为我们考虑的是**能否执行，执行是否合法**的情况。
  * 少数情况下，会写在**后面**。目的就仅仅是**检查更新后的值**的情况，并且默认每次执行合法，都能执行。

* 暂时还没有找到不用long然后也能判断int溢出的好方法。不过下面这种写法可以简单作为参考（这里sum为int）:

  * ``` java
    if ((sum > Integer.MAX_VALUE / 10) || 
        (sum == Integer.MAX_VALUE / 10 && c - '0' > Integer.MAX_VALUE % 10))
        return sign ? Integer.MAX_VALUE : Integer.MIN_VALUE;
    sum = sum * 10 + c - '0';
    ```

  * 这部分判断语句必须写在sum更新前，因为它相等于**预判更新**是否合法（不溢出）。

  * 由于sum表示的是绝对值，所以这里相当于判断sum是否 > Integer.MAX_VALUE。对于正数来说，大于肯定溢出；对于负数，大于的话表示实际值 <= Integer.MIN_VALUE，虽然等于Integer.MIN_VALUE时不算溢出，但还是可以统一按照Integer.MIN_VALUE来处理。

* 时刻注意判断下标的有效性，无论是数组还是字符串。