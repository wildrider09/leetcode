---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.06.Palindrome%20Linked%20List/README_EN.md
---

<!-- problem:start -->

# [02.06. Palindrome Linked List](https://leetcode.cn/problems/palindrome-linked-list-lcci)

[Chinese Version](/lcci/02.06.Palindrome%20Linked%20List/README.md)

## Description

<!-- description:start -->

<p>Implement a function to check if a linked list is a palindrome.</p>

<p>&nbsp;</p>

<p><strong>Example 1: </strong></p>

<pre>

<strong>Input:  </strong>1-&gt;2

<strong>Output: </strong> false

</pre>

<p><strong>Example 2: </strong></p>

<pre>

<strong>Input:  </strong>1-&gt;2-&gt;2-&gt;1

<strong>Output: </strong> true

</pre>

<p>&nbsp;</p>

<p><b>Follow up:</b><br />

Could you do it in O(n) time and O(1) space?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Fast and Slow Pointers + Reverse List

First, we check if the list is empty. If it is, we return `true` directly.

Next, we use fast and slow pointers to find the midpoint of the list. If the length of the list is odd, the slow pointer points to the midpoint. If the length of the list is even, the slow pointer points to the first of the two middle nodes.

Then, we reverse the list after the slow pointer, giving us the second half of the list, with the head node as $p$.

Finally, we loop to compare the first half and the second half of the list. If there are any unequal nodes, we return `false` directly. Otherwise, we return `true` after traversing the list.

The time complexity is $O(n)$, where $n$ is the length of the list. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return true;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode p = slow.next;
        slow.next = null;
        ListNode dummy = new ListNode(0);
        while (p != null) {
            ListNode next = p.next;
            p.next = dummy.next;
            dummy.next = p;
            p = next;
        }
        p = dummy.next;
        while (p != null) {
            if (head.val != p.val) {
                return false;
            }
            head = head.next;
            p = p.next;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
