### 反转链表

- 题目： 输入一个长度为n链表，反转链表后，输出新链表的表头。 
- 思路：略。

```java 
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```



#### 删除链表的倒数第n个节点

- 题目：   给定一个链表，删除链表的倒数第 n 个节点并返回链表的头指针
  例如， 

   给出的链表为: 1→2→3→4→5， n=2.
  删除了链表的倒数第 n 个节点之后,链表变为1→2→3→5

- 解题思路：快慢指针法， 快指针先走 n 步，然后慢指针和快指针一起往后走，直到快指针到达最后一个，这个时候慢指针的位置就是倒数第 n 个节点的前一个，slow.next = slow.next.next 即可

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param n int整型 
     * @return ListNode类
     */
    public ListNode removeNthFromEnd (ListNode head, int n) {
        // write code here
        if (head == null || n <= 0)
            return head;
        ListNode fast = head, slow = head;
        for (int i = 0; i < n; i++) {
            if (fast == null)
                return head;
            fast = fast.next;
        }
        if (fast == null)
            return head.next;
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```



#### 删除有序链表中重复的元素

- 题目： 给出一个升序排序的链表，删除链表中的所有重复出现的元素，只保留原链表中只出现一次的元素。例如： 给出的链表为1→2→3→3→4→4→5， 返回1→2→5
- 解题思路：双指针。pre指针指向已经处理完那部分的最后节点，cur指针指向正要处理的节点。如果cur有重复，则删除所有重复节点。否则，两个指针都往后挪一步。为了统一处理，可以添加一个dummy节点。

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    public ListNode deleteDuplicates (ListNode head) {
        // write code here
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            if (cur.val == cur.next.val) {
                ListNode nextNode = cur.next;
                while (nextNode != null && nextNode.val == cur.val) {
                    nextNode = nextNode.next;
                }
                pre.next = nextNode;
                cur = nextNode;
            } else {
                pre = pre.next;
                cur = cur.next;
            }
        }
        return dummy.next;
    }
}
```





#### 链表中的节点每K个一组翻转

- 题目： 将给出的链表中的节点每 k个一组翻转，返回翻转后的链表。如果链表中的节点数不是  k 的倍数，将最后剩下的节点保持原样。你不能更改节点中的值，只能更改节点本身。要求空间复杂度  O(1)。例如：  给定的链表是1→2→3→4→5  

  对于  k=2， 你应该返回 2→1→4→3→5

  对于  k=3， 你应该返回 3→2→1→4→5



- 解题思路：和普通翻转链表类似，只是这里由多组翻转链表组成，需要用一些变量来记录上一组的尾节点和当前组的头节点。

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode reverseKGroup (ListNode head, int k) {
        // write code here
        if (head == null || k <= 1)
            return head;
        int length = 0;
        ListNode current = head;
        while (current != null) {
            length++;
            current = current.next;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode preTail = dummy;
        current = head;
        ListNode currentTail = current;
        for (int i = 0; i < length / k; i++) {
            ListNode pre = null;
            for (int j = 0; j < k; j++) {
                ListNode next = current.next;
                current.next = pre;
                pre = current;
                current = next;
            }
            preTail.next = pre; //currentHead
            preTail = currentTail;
            currentTail = current;
        }
        if (current != null) {
            preTail.next = current;
        }
        return dummy.next;
    }
}
```





#### 重排链表

- 题目： 将给定的单链表 L： L0→L1→…→Ln−1→Ln
  重新排序为：L0→Ln→L1→Ln−1→L2→Ln−2→…
  要求使用原地算法，不能只改变节点内部的值，需要对实际的节点进行交换。
  例如：
  对于给定的单链表{10,20,30,40}，将其重新排序为{10,40,20,30}.



- 思路：1. 使用快慢指针将链表分为前后两部分，然后截断它们。 2. 翻转第二条链表。 3. 合并两条链表。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null)
            return ;
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode first = head;
        ListNode second = reverseList(slow.next);
        slow.next = null;
        mergeList(first, second);
    }
    
    public void mergeList(ListNode first, ListNode second) {
        while (first != null && second != null) {
            ListNode firstNext = first.next;
            ListNode secondNext = second.next;
            first.next = second;
            second.next = firstNext;
            first = firstNext;
            second = secondNext;
        }
    }
    
    public ListNode reverseList(ListNode head) {
        ListNode cur = head, pre = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```



### 判断链表中是否有环

- 题目：判断给定的链表中是否有环。如果有环则返回true，否则返回false。
- 思路：快慢指针法。快指针每次走两步，慢指针每次走一步，如果有环，则快慢指针肯定会再次相遇。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```



### 两个链表生成相加链表

- 题目：  假设链表中每一个节点的值都在 0 - 9 之间，那么链表整体就可以代表一个整数。 

    给定两个这种链表，请生成代表两个整数相加值的结果链表。 比如，输入：`[9, 3, 7], [6, 3]`， 输出： `[1, 0, 0, 0]`。

- 思路： 

  - 反转两个链表
  - 将反转后的两个链表逐个相加。（注意carry位）
  - 反转相加后的链表

```java
public class Solution {
    /**
     * 
     * @param head1 ListNode类 
     * @param head2 ListNode类 
     * @return ListNode类
     */
    public ListNode addInList (ListNode head1, ListNode head2) {
        // write code here
        if (head1 == null) {
            return head2;
        }
        if (head2 == null) {
            return head1;
        }
        ListNode list1 = reverseList(head1);
        ListNode list2 = reverseList(head2);
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        int carry = 0;
        while (list1 != null || list2 != null) {
            int value1 = list1 == null ? 0 : list1.val;
            int value2 = list2 == null ? 0 : list2.val;
            ListNode temp = new ListNode((value1 + value2 + carry) % 10);
            cur.next = temp;
            cur = temp;
            carry = (value1 + value2 + carry) / 10;
            if (list1 != null) {
                list1 = list1.next;
            }
            if (list2 != null) {
                list2 = list2.next;
            }
        }
        if (carry != 0) {
            cur.next = new ListNode(carry);
        }
        return reverseList(dummy.next);
    }
    
    private ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```



### 链表中环的入口节点

- 题目： 给一个长度为n链表，若其中包含环，请找出该链表的环的入口结点，否则，返回null。
- 思路：快慢指针。快指针每次走两步，慢指针每次走一步，如果有环，快指针肯定会和慢指针相遇。相遇之后再遍历一遍环中的节点，得到环中节点的个数`x`。然后让两个指针从头开始遍历链表，快指针先跑`x`步，然后快慢指针同时每次走一步，直到相遇，相遇的节点就是环的入口节点。

```java
public class Solution {

    public ListNode EntryNodeOfLoop(ListNode pHead) {
        if (pHead == null || pHead.next == null) {
            return null;
        }
        ListNode fast = pHead;
        ListNode slow = pHead;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                break;
            }
        }
        if (fast != slow) {
            return null;
        }
        fast = fast.next;
        int num = 1;
        while (fast != slow) {
            num++;
            fast = fast.next;
        }
        fast = pHead;
        for (int i = 0; i < num; i++) {
            fast = fast.next;
        }
        slow = pHead;
        while (fast != slow) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```



### 合并两个排序的链表

- 题目： 输入两个递增的链表，单个链表的长度为n，合并这两个链表并使新链表中的节点仍然是递增排序的。
- 思路： 略。

```java
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                cur.next = list1;
                list1 = list1.next;
            } else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        if (list1 != null) {
            cur.next = list1;
        }
        if (list2 != null) {
            cur.next = list2;
        }
        return dummy.next;
    }
}
```



### 单链表的排序

- 题目： 给定一个节点数为n的无序单链表，对其按升序排序。
- 思路： 堆排序。分别将链表的节点加入到堆中，然后一个一个poll出来即可。（注意：最后一个节点的next指针要设为null，否则可能会出现环形链表的问题。）

```java
public class Solution {
    /**
     * 
     * @param head ListNode类 the head node
     * @return ListNode类
     */
    public ListNode sortInList (ListNode head) {
        // write code here
        PriorityQueue<ListNode> heap = new PriorityQueue<>((n1, n2) -> n1.val - n2.val);
        while (head != null) {
            heap.add(head);
            head = head.next;
        }
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while (!heap.isEmpty()) {
            cur.next = heap.poll();
            cur = cur.next;
        }
        cur.next = null;
        return dummy.next;
    }
}
```



### 判断一个链表是否为回文结构

- 题目： 给定一个链表，请判断该链表是否为回文结构。
- 思路： 快慢指针法将链表分为两段，然后将一段逆序（反转链表或者使用栈），再逐个对比即可。

```java
public class Solution {
    /**
     * 
     * @param head ListNode类 the head
     * @return bool布尔型
     */
    public boolean isPail (ListNode head) {
        // write code here
        if (head == null || head.next == null) {
            return true;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode fast = dummy, slow = dummy;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        Stack<Integer> stack = new Stack<>();
        ListNode cur = slow.next;
        while (cur != null) {
            stack.push(cur.val);
            cur = cur.next;
        }
        cur = head;
        while (!stack.isEmpty()) {
            if (cur.val != stack.pop()) {
                return false;
            }
            cur = cur.next;
        }
        return true;
    }
}
```



### 链表的奇偶重排

- 题目：   给定一个单链表，请设定一个函数，将链表的奇数位节点和偶数位节点分别放在一起，重排后输出。 注意是节点的编号而非节点的数值。 
- 思路： 将原链表分为两个链表，然后让第一个链表的尾节点指向第二个链表即可。

```java
public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    public ListNode oddEvenList (ListNode head) {
        // write code here
        if (head == null || head.next == null) {
            return head;
        }
        ListNode list1 = head;
        ListNode list2 = head.next;
        ListNode cur1 = list1, cur2 = list2;
        while (cur2 != null && cur2.next != null) {
            cur1.next = cur2.next;
            cur1 = cur1.next;
            cur2.next = cur1.next;
            cur2 = cur2.next;
        }
        cur1.next = list2;
        return list1;
    }
}
```



### 链表内指定区间反转

- 题目： 将一个节点数为 size 链表 m 位置到 n 位置之间的区间反转，要求时间复杂度 O(n)，空间复杂度 O(1)。
- 思路： 将链表分为三段，`1 ~ m -1`、`m ~ n` 、`n + 1 ~ size`。反转中间那段，然后再连起来即可。

```java
public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param m int整型 
     * @param n int整型 
     * @return ListNode类
     */
    public ListNode reverseBetween (ListNode head, int m, int n) {
        // write code here
        if (head == null || head.next == null || m >= n) {
            return head;
        }
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode firstTail = dummy;
        for (int i = 0; i < m - 1; i++) {
            firstTail = firstTail.next;
        }
        ListNode cur = firstTail.next, secondTail = firstTail.next;
        ListNode pre = null;
        int i = m;
        while (cur != null && i <= n) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
            i++;
        }
        firstTail.next = pre;
        secondTail.next = cur;
        return dummy.next;
    }
}
```



### 两个链表的第一个公共节点

- 题目： 输入两个无环的单向链表，找出它们的第一个公共结点，如果没有公共节点则返回空。
- 思路： 定义两个指针，指针1沿着链表1跑完就去跑链表2， 指针2沿着链表2跑完就去跑链表1。只要保证同时起跑并且每次走一步，如果有公共节点，肯定会在公共节点相遇（因为两个指针所有的步数是一样的）。如果没有公共节点，那么两个指针最终都会为null。

```java
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode list1 = pHead1;
        ListNode list2 = pHead2;
        while (list1 != list2) {
            list1 = list1 == null ? pHead2 : list1.next;
            list2 = list2 == null ? pHead1 : list2.next;
        }
        return list1;
    }
}
```



### 合并k个已排序的链表

- 题目： 合并 k 个升序的链表并将结果作为一个升序的链表返回其头节点。  
- 思路： 将这k个链表加入小根堆中（空链表除外）就好了。

```java
public class Solution {
    public ListNode mergeKLists(ArrayList<ListNode> lists) {
        if (lists == null || lists.size() == 0) {
            return null;
        }
        PriorityQueue<ListNode> heap = new PriorityQueue<>((o1, o2) -> o1.val - o2.val);
        for (ListNode list : lists) {
            if (list != null) {
                heap.add(list);
            }
        }
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while (!heap.isEmpty()) {
            ListNode list = heap.poll();
            cur.next = list;
            cur = cur.next;
            list = list.next;
            if (list != null) {
                heap.add(list);
            }
        }
        return dummy.next;
    }
}
```



### 链表中倒数最后k个节点

- 题目：   输入一个长度为 n 的链表，设链表中的元素的值为 ai ，输出一个链表，该输出链表包含原链表中从倒数第 k 个结点至尾节点的全部节点。 如果该链表长度小于k，请返回一个长度为 0 的链表。
- 思路： 略。

```java
public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        if (pHead == null || k <= 0) {
            return null;
        }
        ListNode fast = pHead;
        for (int i = 0; i < k; i++) {
            if (fast == null) {
                return null;
            }
            fast = fast.next;
        }
        ListNode slow = pHead;
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```



