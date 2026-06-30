---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.03.Delete%20Middle%20Node/README_EN.md
---

<!-- problem:start -->

# [02.03. Delete Middle Node](https://leetcode.cn/problems/delete-middle-node-lcci)

[Chinese Version](/lcci/02.03.Delete%20Middle%20Node/README.md)

## Description

<!-- description:start -->

<p>Implement an algorithm to delete a node in the middle (i.e., any node but the first and last node, not necessarily the exact middle) of a singly linked list, given only access to that node.</p>

<p>&nbsp;</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>the node c from the linked list a-&gt;b-&gt;c-&gt;d-&gt;e-&gt;f

<strong>Output: </strong>nothing is returned, but the new linked list looks like a-&gt;b-&gt;d-&gt;e-&gt;f

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Node Assignment

We can replace the value of the current node with the value of the next node, and then delete the next node. This way, we can achieve the purpose of deleting the current node.

The time complexity is $O(1)$, and the space complexity is $O(1)$.

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
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
