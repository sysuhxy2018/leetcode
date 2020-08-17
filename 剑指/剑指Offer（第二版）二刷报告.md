# 剑指Offer（第二版）二刷报告



### [05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

* 如果用正则秒，则必须用**replaceAll(regex, string)**。不能用replace(string, string)。
* 如果用3倍扩增的字符数组，最后char[]转字符串要注意用法：
  - public String(char[] value):把字符数组转成字符串
  - public String(char[] value,int index,int count):把字符数组的一部分转成字符串

### [06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

* 只是倒序打印，不要求翻转。其实最暴力的两次遍历就能解决，第一次先算出长度。

### [10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

* 注意科学计数法默认为double，所以最好还是展开来写，用int表示。

### [11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

* 注意这里是允许**重复**的版本。二分时要**分三类**讨论。

### [12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

* 非法情况包括：矩阵越界、字符不匹配、**已访问**。
* 回溯的话要记得返回前**清理标记**变量。这里可以不用额外设置一个visit矩阵，只要在原来board矩阵上成功访问时设置一个**占位符**，如'#'即可。返回前记得还原就行了。

### [13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

* 属于dfs，不需要回溯。因为只要遍历一个全局的状态，不需要细分路径等。

### [14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

* 注意m > 1，即至少分两段。所以特殊考虑2和3，2返回1，3返回2。

### [14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

* 大数据。负责累乘的ans要设为long，而且不能图方便，直接 *= 4, *= 2，每一步都要记得**求余**。

### [15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

* 一个方法是每次一位位往右移，然后每次和1与运算即可。特别注意这里右移是**无符号右移**>>>，否则如果遇到负数，带符号右移>>会导致最后结果永远不可能为0，死循环。
* 上面的方法相当于从左往右遍历，还有一种是从右往左。每次n = n & (n - 1)，消去最右边的1，直到最后全部为0。
* 注意位运算的**优先级很低**，一定要带括号运行。

### [16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

* 快速幂模板，整体和15的第一种方法很类似。
* n负数时要转成long来做，注意**不是long a = -n**，这样a还是溢出后的结果。要先赋值给a，再对a取相反数。

### [18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

* 方法一：尾插法。记得最后**要断尾**。
* 方法二：定位 + 修改链接。这个的话最好用一个dummy头节点，便于遍历并记录pre的位置。

### [19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

* dp。dp(i, j)表示[i, m)和[j, n)是否匹配，是一个从后往前的dp。
* 注意初始化，s为空时，p要匹配，则至少要求第二位为 '*'，第一位**没有任何要求**。

### [20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

* 判定条件非常多：
  * 首先要trim()，然后就是排除空串的情况，空串为false。
  * 然后依次考虑符号，整数位，小数点，小数位，指数e/E，指数位。
  * **小数点前后不能没有数字**，至少有一边要有，如果没有就直接返回false。
  * **指数e/E前面不能没有数字或者小数点**
  * **指数e/E里面可能有符号，跳过就行**
  * **指数e/E后面一定要有整数部分**

### [21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

* 注意题目没有要求要保持**原来的相对顺序**。
* 如果要保持顺序，则遍历两次或者稳定冒泡（会超时）
* 最佳的方法是双指针。就是从左边找偶数，右边找奇数，然后交换即可，以此类推。

### [29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

* 维护上下左右4个指针。
* 遍历方向是：第一行（包含两端），最后一列（不包两端），最后一行（包含两端），第一列（不包含两端）。
* 注意行/列**重合**的情况就行了。

### [31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

* 栈模拟。模拟的过程是每次先压入一个pushed，然后检查并弹出多个popped。**先压后弹**，顺序不要弄错了。

### [34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

* 只要结点不为空，就可以**先更新**path和sum，但是一定要等到**叶子结点**时再去判断sum是否满足。

### [35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

* 三步走：
  * 在每个原结点后面插入一个新的copy结点。
  * 根据原结点，修改copy结点间的random指向。
  * 分离原链表和新的copy链表。这步很关键，一定要**完全恢复**原结点间的next指向。

### [36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

* 注意这里要求不能创建任何新的结点，但是可以**申请额外空间**。
* 最简单粗暴的方法就是inorder遍历，然后把结点放到数组里。然后前后**两两互指**，最后头尾互指即可。
* 递归。inorder遍历时记录一个**前驱结点**pre，那么我们可以不用保存记录，就可以直接相邻的两个互指。另外，还要额外维护一个头结点head，要不断更新。最后记得让head和pre(inorder遍历结束后pre会处于最后一个结点，即tail)互指。

### [37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

* 用层次遍历来完成。
* serialize比较简单，就是层次遍历。但注意这里是**带null结点的层次遍历**。和原来的没什么区别，就是要判断一下poll出的结点tmp是否为null，如果不是，才往队列里插入tmp.left和tmp.right，并且这里**不需要判断左右儿子**是否为null，null也照样插入。还有就是结点的值要用**分隔符**，否则连在一起分不清楚。
* deserialize和层次遍历很类似，也是需要一个队列，并且用root结点来启动。另外维护一个**遍历指针 i **。每次从队列里弹出一个结点p，然后看 i 对应位置是否为null，如果不是则新建一个结点并将其入列，该结点为p的左儿子，最后 i++ 更新；右儿子的处理方式同理。最后返回事先记录好的root结点即可。

### [38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

* 去重全排列。
* 从剩下的里面选，选过的元素swap到前面。
* 去重指每轮选择，已选过相同的就不再选了，**用set判定是否选过**。

### [40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

* （大根）堆，用PriorityQueue可以秒，但它默认是小根堆，所以要重写一下比较器。
* 快排的partition思想 + 二分分治。
* 拓展：如果是求最大的k个数，则用（小根）堆或者上面的快排（需按照从大到小划分）。

### [41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

* 大根堆 + 小根堆。大根堆存小的那半部分，小根堆存大的那半部分，不要弄反了。
* 保持两个堆**平衡**，优先存大堆。只要记住：往哪个堆插入数据前，先通过**另一个堆“加工”**一下再重新插入。不需要又分奇偶去讨论什么的，太麻烦。

### [43. 1～n整数中1出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

* 递归分治。这个[解法](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/javadi-gui-by-xujunyi/)是我见过最秀、最容易理解的。基本思路是将 n 按10000...0来拆分。

### [44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

* 注意这题一个tricky的地方，第n位**从0开始**。所以相当于是第n + 1位。
* **排除0**，因为它破坏了规律：1位9个，2位90个，3位900个......。所以其实n + 1 - 1 = n，无差别。
* 然后就去迭代，统计sum。直到sum + d * p >= n 为止。这个时候可以明确第 n 位对应 d 位数，那么 (n - sum) / d就表示是d位数里的第几个（从pow(10, d - 1)开始算起）。(n - sum) % d就表示它是这个数的第几位。如果 **% d为0**，说明n这个数刚好是(n - sum) / d对应这个数的最后一位；否则，是(n - sum) / d的**下一个数**开始去数第几个。
* 还有一个要注意的地方，由于sum和p可能会很大，所以都设置成 **long** 是最保险的。

### [45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

* 贪心排序。排序的话返回值，看你怎么排，如果是a，b来排，则返回负数；b，a则为正数。

### [48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

* 滑动窗口 + 哈希表。当出现重复字符时，才要更新窗口起始位置 j，然后不论是否重复都按该字符新的位置**加入map**。

### [49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

* 比较简单直接的方法：BFS。因为所有丑数都可以基于已有丑数乘以质因子2, 3, 5来获得。有几点要注意：
  * 这里BFS用优先队列，因为涉及到大小关系。
  * 基本模式就是弹出，添加关联的结点。**添加时一定要去重**，一般用Set来实现。
  * 不能在插入后就计数，因为这个数不一定是按顺序的，要经过优先队列调整后才能确保有序。所以只能在**弹出时计数**。
  * 涉及到比较大的数，用Long。
* dp。维护三个候选位置a, b, c。那么dp[i]就从这三个里面衍生得到，取最小的结果。从哪个得到那这个**位置后移**。因为可能会出现相同的结果，所以对每个候选都要**分别判断**。

### [50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

* 哈希表。遍历两次，第一次记录是否重复；第二次找不重复的，**找到就返回**即可。

### [51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

* 归并模板。注意：
  * merge分成[i, m]和[**m + 1**, j]两块，都是**闭区间**。
  * 逆序对出现在**a[p] > a[q]**的情况（另一种情况是a[p] <= a[q]），此时a[p, m]都是 > a[q]的，共计 **m - p + 1** 个。

### [53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

* 可以遍历，高斯求和或者用异或。但是没用到题目的条件**"递增排序"**
* 递增一般容易联想到用二分查找。实际上我们要找的是**第一个nums[m] != m**的位置。如果不等，记录后继续往左找；否则，说明前面都没有问题，往右找。
* 最后注意一个corner case：如果找不到，说明缺的那个数就是最大的那个数，即n。

### [54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

* 中序遍历就行了。注意是**第k大**而不是第k小，所以稍微变换一下，**先右子树再左子树**。

### [56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

* 分组异或。得到 a ^ b 后，找出某个位是 1 的作为掩码。1 说明 a 和 b这一位不一样。再次遍历，将都是 1 的异或在一起，都是 0 的异或在一起，最后就只剩下 a 和 b 了。

### [56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

* 位运算逐位统计。维护一个32大小的数组，遍历所有数的每一个bit，并将其累加到数组对应项上。
* 然后数组元素 **% 3**，就得到了只出现一次的那个数字的所有bits。
* 最后组装得到结果。组装时直接看**count[i]是否为1**，是就累加位移码tmp。
* 拓展：上述方法可以解决其他数字出现 m 次的问题

### [57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

* Two Sum。可以用哈希表。
* 这里默认是升序排列，则可以直接用双指针。

### [57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

* 注意题目要求，序列都是**正整数**且至少**两个**。可以先排除target <= 2的情况
* 滑动窗口，从最小的有效窗口**[1, 2]**开始。动态变化窗口的尺寸。并且由于是连续的，所以计算窗口内的和可以直接高斯求和。最后当窗口右边界 j == target 时终止，即最大到 target - 1。

### [59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

* 方法一：优先队列。就每次**先插入**，如果队列大小 == k就弹出最大值，然后删除窗口最前面的值。
* 方法二：最大列（非严格递减），用Deque实现。也是**先插入**，但是插入前要**从后往前移除**比nums[i]要小的，最后再加入nums[i]。插入后，如果窗口成型(i >= k - 1)，则添加队首到结果里，然后只要要删除的值不在队首，就不用管他，因为不会影响最大值。如果在队首，就要**移除队首**。
* 滑动窗口一般会有一个生成窗口的过程，就每次**先插入，再考虑窗口是否成型**即可。

### [59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

* 和min_stack类似，要一个队列存数据，一个存最大值。
* 如果上题59-I理解了，不难想到用Deque维护一个最大列。push_back时先移除再插入；pop_front时看pop的元素是否和队首一样，一样就移除队首。

### [60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

* dp。公式不难写，主要是很难想。
* dp(i, j)表示 i 个骰子总点数为 j 的情况数。则dp(i, j) += dp(i - 1, j - k)，k∈[1, 6]。所以一个3重for循环就能完成，注意要判断 **j >= k**，避免越界。另外，这里 i 就取 [0, n]，j 取[0, 6 * n]。
* 最后就是将每种情况数除以总情况数得到概率，注意n个骰子的点数只能是**[n, 6 * n]**，合计5 * n + 1种情况。

### [61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

* 注意题目没说是一副牌，所以可能有很多张A，很多张大小王等等。
* 排除王，要检查是否有重复（用Set完成），还要记录**最小**和**最大**的牌。
* 大小牌**差值 < 5**即可构成顺子。

### [62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

* 约瑟夫环问题。用循环数组模拟可做。注意不要一个个去计数遍历，会很耗时。直接定位**删除的位置**即可。i = (i + m - 1) % size。另外，实际操作时用**ArrayList**性能相比LinkedList更好，后者会超时。
* 递推公式推导。f(n - 1, m)是从0号开始的，而f(n, m)第一次删除后是m号开始的（按最开始的编号算，不重新改编下标），所以相当有一个m的偏移。即f(n, m) = f(n - 1, m) + m，为了确保不溢出，还要 **% n**（也是按最开始的 n 个数来求余）。

### [64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

* 利用&&或者||运算符，达到一个分支选择（**短路**）的效果。但注意两边都必须是布尔表达式，如 n > 0 && (n += sum(n - 1)) > 0。完成后，返回n即可。

### [65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

* 递归 + 位运算。
  * 首先不考虑进位，则单纯**相加可以用异或**实现 a ^ b
  * 然后考虑进位，只有都是1才会往前进位，所以用 a & b得出1的，然后因为要向前进，所以要**左移一位**。即(a & b) << 1
  * 将上面两个用add方法加起来即可。递归的边界情况是a或b其中一个**为0**。

### [67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

* 分三块：跳过连续空格，排除符号位，统计连续整数。
* 注意溢出的判断，可以简单粗暴用long，但是并不好。最好的办法是在溢出前就判断，所以我们要在sum每次**更新前先判断**。
* MAX和MIN的溢出判断略有不同，一个个位>'7'，一个个位>'8'，当然前提是sum已经**等于border**（border就是MAX / 10那部分，MIN和MAX都一样，不考虑正负）。如果已经 > border，则可直接判断溢出。



