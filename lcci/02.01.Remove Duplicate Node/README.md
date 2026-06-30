---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/02.01.Remove%20Duplicate%20Node/README_EN.md
---

<!-- problem:start -->

# [02.01. Remove Duplicate Node](https://leetcode.cn/problems/remove-duplicate-node-lcci)

[Chinese Version](/lcci/02.01.Remove%20Duplicate%20Node/README.md)

## Description

<!-- description:start -->

<p>Write code to remove duplicates from an unsorted linked list.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: [1, 2, 3, 3, 2, 1]

<strong> Output</strong>: [1, 2, 3]

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: [1, 1, 1, 1, 2]

<strong> Output</strong>: [1, 2]

</pre>

<p><strong>Note: </strong></p>

<ol>
	<li>The length of the list is within the range[0, 20000].</li>
    <li>The values of the list elements are within the range [0, 20000].</li>
</ol>

<p><strong>Follow Up: </strong></p>

<p>How would you solve this problem if a temporary buffer is not allowed?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

We create a hash table $vis$ to record the values of the nodes that have been visited.

Then we create a dummy node $pre$ such that $pre.next = head$.

Next, we traverse the linked list. If the value of the current node is already in the hash table, we delete the current node, i.e., $pre.next = pre.next.next$; otherwise, we add the value of the current node to the hash table and move $pre$ to the next node.

After the traversal, we return the head of the linked list.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the linked list.

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
    public ListNode removeDuplicateNodes(ListNode head) {
        Set<Integer> vis = new HashSet<>();
        ListNode pre = new ListNode(0, head);
        while (pre.next != null) {
            if (vis.add(pre.next.val)) {
                pre = pre.next;
            } else {
                pre.next = pre.next.next;
            }
        }
        return head;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
