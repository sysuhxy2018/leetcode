# LeetCode 热题 HOT 100二刷报告



### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

* 参考[链接](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/)的解法三。个人感觉解法三相比解法四要更清晰易懂些。
* 首先将问题转化成求两个数组中第K个数的问题，这里K取(m + n + 1) / 2和(m + n + 2) / 2，刚好就是中间两个数。
* 对于上面的问题，用递归解决。
  * 首先让长度上数组A <= 数组B（如果不是，将**参数的位置换一下**，再调用一次方法即可，），则如果**A为空**，剩下就可以直接从B里找第K个。
  * 另外，如果**K == 1**，则可以直接比较数组A和数组B的第一个元素，取**较小**的那个。
  * 对于一般情况，取A和B的**第K / 2个**元素来比较（如果没有，就取到**最末尾**那个元素，可以用Math.min(K / 2, len）来获取长度）。**小的**那一方，就能排除掉数组的**前半部分（包括刚才比较的元素）**。根据排除掉的数量，更新K和小的那一方数组的范围即可。

### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

* corner case: 空串，直接返回**空数组**即可。

### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

* 首先定位倒数第N个节点，与此同时维护一个**pre指针**，便于后续删除。
* 因为涉及到pre指针，所以最好开始加一个**dummy**头部结点。

### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

* 生成过程中为了确保有效，需要 right < left 或者 **right == left** （等于时只能添加左括号）

### [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

* 比较简单直接的方法是先考虑合并2个有序链表，然后迭代即可。最开始的ans列表设置为**null**，后面每合并一次就**更新ans**（头结点）。
* 更有效的方法是维护一个K个大小的小根堆。注意小根堆存放的是**ListNode**，而不是Integer。
  * 整个过程中，要确保不会插入null，多加判断即可。
  * 首先初始化堆，遍历lists，将头结点都插入。
  * 然后只要堆不为空，每次弹出最小的，如果它**下一位**还存在，就插入下一位来补充空缺。

### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

* 类似于删除最少无效括号，但这样得到的其实是**子序列**，而不是**子串**的长度。
* 但我们可以照搬上面的思路。就是用stack，**存左括号的下标**，这样在第一次遍历时就能排除掉那些非法的右括号。这里用一个无效的**占位字符**'#'来表示非法。
* 然后清空stack，排除掉非法的左括号。同样用'#'占位。
* 第二次遍历，计算**连续的**非'#'字符即可。

### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

* 回忆一下，找旋转数组最小值的时候，用的是nums[m]和nums[r]直接比较，但这里又多出一个target怎么办呢？
* 首先确定有序区间。如果nums[m] < nums[r]，则[m, r]有序；否则，[l, m]有序。然后就是判断target**是否在有序区间**里，如果是，就将二分范围缩小至有序区间；不是，就排除有序区间，定位到剩下的区间。

### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

* 关键是如何去重，需要做到两点：
  * 对candidates排序
  * 比如说在[i, n)中选了j，那么下次就从**[j, n)**中选（因为可以任选无数次，所以下次可以继续从 j 开始）
* 这样确保所有组合都是有序的，就不会出现重复的了。

### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

* 左右各遍历一次找出每根柱子左右的**极大值**。
* 根据木桶的短板效应，左右极大值**取较小的**，然后记得要 **> height[i]** 时才累加差值，不然会把负的也算进去。

### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

* 如果不想单独讨论最后一行/列的话，可以多开一行/列。然后将dp数组初始化为**Integer.MAX_VALUE**。一般情况讨论时，就单独判断一下(m - 1, n - 1)这个位置即可。（怎么感觉更麻烦了。。。）

### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

* 仍然习惯性用dp(i, j)表示[i, m)和[j, n]。
* 初始化 i = m 和 j = n 的情况，即一方为空。
* 一般情况，由于取最小值，所以dp(i, j)赋值为**MAX_VALUE**。 
  * 先看 i 和 j 位置的字符**是否相等**，是就**不用操作**，直接取dp(i + 1, j + 1)。
  * 不是，那就**操作**。注意一旦操作，就要 **+ 1**，写的时候太粗心容易给漏了。
    * 插入和删除其实本质一样，都是可以让一个忽略首字符，然后继续和另一个比较。所以是Math.min(dp(i, j + 1), dp(i + 1, j)) + 1
    * 替换就更简单了，直接是dp(i + 1, j + 1) + 1

### [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

* 方法一：计数排序。记录0，1，2的数目，然后依次覆盖即可。
* 方法二：双指针。l，r表示0和2可填的位置（从**最左**和**最右**算起）。然后遍历一次分类讨论即可。
  * 如果是1，直接跳过，i++
  * 如果是0，交换 i 和 l 位置，l++，i++。因为 l 在 i 的左边，已经遍历过了，所以不管交换过来什么数字，都可以直接跳过。
  * 如果是2，交换 i 和 r 位置，r--，**i不动**。因为 r 在 i 的右边，交换过来的数字也是在右边，所以不知道情况如何，**不能跳过**。
  * 另外还要注意while的条件是 **i <= r**，而不是 i < n，因为要确保 i 不会遍历到已经填好的位置。

### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

* 首先不考虑最小的问题，考虑字符串是否包含另一个字符串的所有字符。不推荐用HashMap，太慢。开两个**128大小的数组**就行了，每次只要遍历一次，一旦出现b[i] > 0 && a[i] < b[i]的情况，就说明a无法包含b，直接返回false。
* 然后主体用滑动窗口法。这里我们是**先添加内容，后检查窗口**；因为如果先检查，后添加的话，最后一次添加完了，还要在while后面多检查一次。况且一开始窗口为空，添加后才开始有内容。
  * 如果窗口无效，则直接 j++
  * 如果有效，则 i++，同时检查更新长度。
    * 这部分的写法有两种，一种是**不写内嵌while**，而是把 j 的更新完全交给最外面的while实现，但这样可能有一个问题。就是 i 在变化但是 **j 不变**，也就是会重复添加同样的 s.charAt(j) 。所以这里要用一个boolean数组，表示该位置是否添加过了，如果是就不再重复添加了。
    * 另一种是**内嵌while**，直到窗口无效为止。这里注意while的条件，要保持 **i <= j**。内嵌while结束后，由于窗口无效，和最上面的情况一样，记得要 **j++**。
* 另外注意其他几个点：
  * len初始设置为MAX_VALUE（其实只要比s.length()大就行，但**不能设置为s.length()**，否则无法判断是否能够找到）。
  * 可以维护两个下标，l，r。这样到最后取[l, r]子串即可，就不用每次都substring记录子串了。

### [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

* dfs边界情况比较多，不要遗漏。先考虑**成功搜索**的，返回true；再考虑不成功的，返回false。

### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

* 可以类比[42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)。也是求一个柱子左右两边的阈值。这里求的是左右两边第一个比heights[i]低的柱子。所以维护两个数组**left和right**。
* 先看左边的，维护一个单调最小stack，注意存的是**下标**。每次找之前先弹出 >= heights[i]，直到为空或者栈顶 < heights[i]。然后我们就可以记录了。如果为空，设置无效的下标 -1。
* 右边的和左边的类似，不过由于对称性，要从右往左遍历。如果找不到，就设置无效下标 n。这里下标的设置都是有讲究的，因为我们真正要的区间是[left + 1, right - 1]。所以这样设置。
* 再遍历一次，用高 * 宽得出面积，更新最大面积即可。

### [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

* [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)变形。可以维护一个heights数组，用于统计每一列的高度。
* 从上往下遍历，动态更新heights[]，如果遇到1，就继续 + 1；遇到0，就**重置为0**。然后就相当于每一行调用一次84题的方法，一模一样。

### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

* 发现一个超神的[解法](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/die-dai-jie-fa-shi-jian-fu-za-du-onkong-jian-fu-za/)，无论前中后，用模板（基于前序）都能秒。
* 中序的话，就是不要一开始就加入ans，而是等到左子树都遍历完了，**弹出的时候添加**就好了。
* 补充：后序的话，先考虑先遍历右子树，再左子树，则得到根，右，左。最后**reverse结果**即可。

### [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

* dp[i] 表示以 i 个结点构成的二叉搜索树有多少。其实很好理解，只要排除掉根后，考虑**左子树有几个结点**，剩下的就是右子树了，而左子树可以为0 ~ i - 1个。这里就可以更新累加了。

### [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

* 类似于BST展开为循环双向链表。不过那个是中序遍历。
* 方法一：前序遍历并储存所有结点，然后中间两两相连（这里我们统一**right为指向**，left记得重置为null）
* 方法二：递归。利用**后序遍历**。这里肯定是要后序的，因为flatten方法没有返回值，只能先完成左右子树，再处理。这里我们用pre记录**前驱结点**。如果是一般后序，pre得到的是cur的右儿子，但这明显不是我们要的，root要连接的是左儿子。所以可以**先右后左**，这样pre就是左儿子了。然后root.right = pre; roo.left = null即可。注意返回前记得更新pre。
* 有人问，左儿子为什么不直接root.left呢。因为后序遍历，前面已经把除root外的其他结点的left设为null了，所以获取不到。

### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

* 用HashSet，每次只找比自己大的，直到没有为止。
* 为了避免重复查找，已经出现过在我们遍历过的序列的，就不要重复去找序列了。即当**没有比自己小的**时候，我们再考虑去计算序列。

### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

* 直觉来说，空串时应该为false，无法拆分。但是显然这里dp[n]我们要设置为**true**，不然结果就不对了。

### [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

* 一般LRU不会只存一个数，而是**<K,V>**对。数的话如果有重复，那就不能确定是使用哪个了。所以这里除了Deque（链表）外，还要一个HashMap。Deque**存key**就行了。
* get，如果命中，则**视为访问**，要提到队首。
* put，如果命中，相当于修改，不会影响size，所以不考虑溢出；否则，要先检查size是否已经满了，如果是，先删除队尾，然后再插入。删除时，map里的元素也要跟着删除。

### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

* 就按归并排序写。有边界情况，有二分（拆成两个子链表），二分后各自排序再merge。

### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

* 又粗心了，prod初始值不是MIN_VALUE，而是**nums[0]**。

### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

* 拓扑排序。我们需要**邻接线性表**。就是记录每个点和它对应指向的点的列表，一般是List<List\<Integer>>这种。
* 然后还要记录每个点的 in-degree（入度），这个用一个数组就行了。
* 拓扑的话，有两种算法：BFS和DFS。个人觉得BFS的更好理解些。
  * 首先初始化队列，将所有**入度为0**的入列。然后开始弹出、添加。弹出一个点后，cnt++，然后遍历它的邻居（用邻接线性表）。由于这个点被删了，所以导致邻居的入度都**减一**。如果减为0，就可以**添加**到队列。（这里不需要考虑重复添加过的问题）。
  * 最后cnt就是能化为0的点的数目，也就是如果cnt == n，则说明是DAG，存在拓扑序列。

### [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

* 字典树。本质上是一个多叉树，其结点Node只需存两个变量
  * Node结点的数组，大小是**字符集的大小**。由于这里是小写，所以26就足够了。
  * 不需要存具体的数据，只需存储一个表示**是否是单词**的布尔变量，默认为false。注意它不是表示是否是叶子。
* 这里首先有一个root结点，它没有实际作用，是用于引导遍历的。
* insert：从root开始，检查数组中字符对应位置是否为null，如果**是null才添加**新结点。然后位移到该结点。最后记得将p的 isWord 设为 **true**。
* search/startsWith：比较类似。也是从root开始遍历，如果遇到数组中字符对应位置为null，则说明查找失败。search最后要确保 p 的 isWord 为**true**；startsWith则没有任何要求。

### [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

* 可以按[85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)来做，只不过计算面积时要取min(宽，高)的平方。但是这样简单问题就复杂化了。
* dp(i, j)表示以(i, j)位置为右下角的**最大正方形边长**。首先matrix(i, j)**必须要为1**，如果是0，就肯定不存在了。 
  * 对于第0行/列来说，最大边长只能为1，所以如果matrix(i, j) == 1，则dp(i, j) = 1。
  * 一般情况，有这样一个递推公式，如果matrix(i, j) == 1，则dp(i, j) = Min(dp(i - 1, j), dp(i, j - 1), dp(i - 1, j - 1)) + 1。即检查左边，上边和左上**3个位置**。具体证明略。
* 最后side从0开始，注意初始化情况时side也要考虑**更新**。

### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

* 无语了，这么简单的题错漏百出，完全不在状态。
* 首先while遍历时，结点指针又忘记位移更新了，导致超时。
* 然后是后半段链表翻转后，遍历指针还是从slow开始，明显要从**翻转后的head**开始。

### 253. Meeting Rooms II

* 首先对所有区间**按开始顺序**排序，这样保证后面遍历时是按开始顺序的。这里排序可以直接用Arrays.sort，对象是int[]。可以不用先存List<int[]>，再Collections.sort。
* 然后维护一个优先队列，**按结束时间**排序。遍历区间，如果队列为空或者**最早结束的**（也就是队首）> 当前区间的开始时间，就必须多开一个房间，区间放入队列；否则，我们可以在队首结束后，继续使用它所在的房间，也就是**弹出队首**，再添加新区间。
* 最后队列的大小就是所需房间数。

### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

* dp。意外地比较好写。
* dp(i)时，如果 i 是完全平方数，直接得到 1；否则，从[1, (int)sqrt(i)]中枚举得到一个完全平方数，减去该完全平方数，看剩下的dp哪个最小即可。

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

* [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)的弱化版。有两种写法：把0放到右边；或者把**非0放到左边**。但是第一种写法**不能保证顺序**，只能用后一种。
* 还有就是用冒泡排序。非零的 < 零。

### 

