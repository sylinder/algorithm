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

- 解题思路：快慢指针法， 快指针先走 n 步，然后慢指针和快指针一起往后走，指导快指针到达最后一个，这个时候慢指针的位置就是倒数第 n 个节点的前一个，slow.next = slow.next.next 即可

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

