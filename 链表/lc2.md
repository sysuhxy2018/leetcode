### 代码

``` java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode h1 = l1;
        ListNode h2 = l2;
        ListNode head, tail;
        head = tail = new ListNode(-1);
        int carry = 0;
        while (h1 != null && h2 != null) {
            int a = h1.val, b = h2.val;
            int sum = a + b + carry;
            ListNode tmp = new ListNode(sum % 10);
            tail.next = tmp;
            tail = tmp;
            carry = sum / 10;
            h1 = h1.next;
            h2 = h2.next;
        }
        while (h1 != null) {
            int a = h1.val;
            int sum = a + carry;
            ListNode tmp = new ListNode(sum % 10);
            tail.next = tmp;
            tail = tmp;
            carry = sum / 10;
            h1 = h1.next;
        }
        while (h2 != null) {
            int b = h2.val;
            int sum = b + carry;
            ListNode tmp = new ListNode(sum % 10);
            tail.next = tmp;
            tail = tmp;
            carry = sum / 10;
            h2 = h2.next;
        }
        if (carry > 0) {
            ListNode tmp = new ListNode(carry);
            tail.next = tmp;
            tail = tmp;
        }
        tail.next = null;
        return head.next;
    }
}

// 简化版
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head, tail;
        head = tail = new ListNode(-1);
        int c = 0;
        while (l1 != null || l2 != null) {
            int a = l1 == null ? 0 : l1.val;
            int b = l2 == null ? 0 : l2.val;
            int sum = a + b + c;
            
            ListNode tmp = new ListNode(sum % 10);
            tail.next = tmp;
            tail = tmp;
            
            c = sum / 10;
            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
        }
        if (c > 0) {
            ListNode tmp = new ListNode(c);
            tail.next = tmp;
            tail = tmp;
        }
        //tail.next = null;
        return head.next;
    }
}
```



### 思路

总体上属于链表的遍历 + 尾插（带头结点）。下面以简化版为例讲解一下思路。

* 维护一个变量c，表示进位，初始化为0。
* 遍历链表l1和l2，**直到 l1 和 l2 都为null**跳出循环，即我们一直遍历完较长的那个链表。
  * 设置加数。如果一方为null，则加数设置为0，否则用.val操作获取。
  * 构造新结点，并用尾插法插入新链表。
  * 更新进位c。更新遍历指针l1和l2，如果为null，则不变，否则更新为.next。
* 最后可能还剩进位c （如果c > 0时），记得要插入新链表。

一些注意事项：

* tail最后可以不需要断尾，因为每个新结点默认next都是null。
* 再次强调，while里不能够遗漏下标，**指针的更新**。一定要养成好的习惯，先在循环最后写上一个更新语句，免得后面忘了。
* 不要遗漏最后的进位c。

然后补充一下对题意的说明。题目给出的l1和l2都是按照从低位到高位的顺序，并不是通常的从高位到低位的顺序。所以我们可以不需要反转链表就可以直接遍历操作。另外，最后的结果新链表也是按照从低位到高位的顺序。所以我们才直接用尾插而不是头插。