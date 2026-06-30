---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.05.Sum%20Lists/README_EN.md
---

<!-- problem:start -->

# [02.05. Sum Lists](https://leetcode.cn/problems/sum-lists-lcci)

[Chinese Version](/lcci/02.05.Sum%20Lists/README.md)

## Description

<!-- description:start -->

<p>You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1&#39;s digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.</p>

<p>&nbsp;</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>(7 -&gt; 1 -&gt; 6) + (5 -&gt; 9 -&gt; 2). That is, 617 + 295.

<strong>Output: </strong>2 -&gt; 1 -&gt; 9. That is, 912.

</pre>

<p><strong>Follow Up:&nbsp;</strong>Suppose the digits are stored in forward order. Repeat the above problem.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>(6 -&gt; 1 -&gt; 7) + (2 -&gt; 9 -&gt; 5). That is, 617 + 295.

<strong>Output: </strong>9 -&gt; 1 -&gt; 2. That is, 912.

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

We traverse two linked lists $l_1$ and $l_2$ simultaneously, and use a variable $carry$ to indicate whether there is a carry-over currently.

During each traversal, we take out the current digit of the corresponding linked list, calculate the sum of them and the carry-over $carry$, then update the value of the carry-over, and finally add the value of the current digit to the answer linked list. The traversal ends when both linked lists have been traversed and the carry-over is $0$.

Finally, we return the head node of the answer linked list.

The time complexity is $O(\max(m, n))$, where $m$ and $n$ are the lengths of the two linked lists respectively. We need to traverse all positions of the two linked lists, and it only takes $O(1)$ time to process each position. Ignoring the space consumption of the answer, the space complexity is $O(1)$.

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        int carry = 0;
        ListNode cur = dummy;
        while (l1 != null || l2 != null || carry != 0) {
            int s = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carry;
            carry = s / 10;
            cur.next = new ListNode(s % 10);
            cur = cur.next;
            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
        }
        return dummy.next;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
