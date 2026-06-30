---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.02.Kth%20Node%20From%20End%20of%20List/README_EN.md
---

<!-- problem:start -->

# [02.02. Kth Node From End of List](https://leetcode.cn/problems/kth-node-from-end-of-list-lcci)

[Chinese Version](/lcci/02.02.Kth%20Node%20From%20End%20of%20List/README.md)

## Description

<!-- description:start -->

<p>Implement an algorithm to find the kth to last element of a singly linked list.&nbsp;Return the value of the element.</p>

<p><strong>Note: </strong>This problem is slightly different from the original one in the book.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong> 1-&gt;2-&gt;3-&gt;4-&gt;5 and <em>k</em> = 2

<strong>Output:  </strong>4</pre>

<p><strong>Note: </strong></p>

<p>k is always valid.</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Two Pointers

We define two pointers `slow` and `fast`, both initially pointing to the head node `head`. Then the `fast` pointer moves forward $k$ steps first, and then the `slow` and `fast` pointers move forward together until the `fast` pointer points to the end of the list. At this point, the node pointed to by the `slow` pointer is the $k$-th node from the end of the list.

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
    public int kthToLast(ListNode head, int k) {
        ListNode slow = head, fast = head;
        while (k-- > 0) {
            fast = fast.next;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow.val;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
