---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.04.Partition%20List/README_EN.md
---

<!-- problem:start -->

# [02.04. Partition List](https://leetcode.cn/problems/partition-list-lcci)

[Chinese Version](/lcci/02.04.Partition%20List/README.md)

## Description

<!-- description:start -->

<p>Write code to partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x. If x is contained within the list, the values of x only need to be after the elements less than x (see below). The partition element x can appear anywhere in the &quot;right partition&quot;; it does not need to appear between the left and right partitions.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong> head = 3-&gt;5-&gt;8-&gt;5-&gt;10-&gt;2-&gt;1, <em>x</em> = 5

<strong>Output:</strong> 3-&gt;1-&gt;2-&gt;10-&gt;5-&gt;5-&gt;8

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Concatenating Lists

We create two lists, `left` and `right`, to store nodes that are less than `x` and nodes that are greater than or equal to `x`, respectively.

Then we use two pointers `p1` and `p2` to point to the last node of `left` and `right` respectively, initially both `p1` and `p2` point to a dummy head node.

Next, we traverse the list `head`. If the value of the current node is less than `x`, we add the current node to the end of the `left` list, i.e., `p1.next = head`, and then set `p1 = p1.next`; otherwise, we add the current node to the end of the `right` list, i.e., `p2.next = head`, and then set `p2 = p2.next`.

After the traversal, we point the tail node of the `left` list to the first valid node of the `right` list, i.e., `p1.next = right.next`, and then point the tail node of the `right` list to a null node, i.e., `p2.next = null`.

Finally, we return the first valid node of the `left` list.

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
    public ListNode partition(ListNode head, int x) {
        ListNode left = new ListNode(0);
        ListNode right = new ListNode(0);
        ListNode p1 = left;
        ListNode p2 = right;
        for (; head != null; head = head.next) {
            if (head.val < x) {
                p1.next = head;
                p1 = p1.next;
            } else {
                p2.next = head;
                p2 = p2.next;
            }
        }
        p1.next = right.next;
        p2.next = null;
        return left.next;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
