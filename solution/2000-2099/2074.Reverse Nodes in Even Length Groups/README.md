---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/README_EN.md
rating: 1685
source: Weekly Contest 267 Q2
tags:
    - Linked List
---

<!-- problem:start -->

# [2074. Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups)

[Chinese Version](/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/README.md)

## Description

<!-- description:start -->

<p>You are given the <code>head</code> of a linked list.</p>

<p>The nodes in the linked list are <strong>sequentially</strong> assigned to <strong>non-empty</strong> groups whose lengths form the sequence of the natural numbers (<code>1, 2, 3, 4, ...</code>). The <strong>length</strong> of a group is the number of nodes assigned to it. In other words,</p>

<ul>
	<li>The <code>1<sup>st</sup></code> node is assigned to the first group.</li>
	<li>The <code>2<sup>nd</sup></code> and the <code>3<sup>rd</sup></code> nodes are assigned to the second group.</li>
	<li>The <code>4<sup>th</sup></code>, <code>5<sup>th</sup></code>, and <code>6<sup>th</sup></code> nodes are assigned to the third group, and so on.</li>
</ul>

<p>Note that the length of the last group may be less than or equal to <code>1 + the length of the second to last group</code>.</p>

<p><strong>Reverse</strong> the nodes in each group with an <strong>even</strong> length, and return <em>the</em> <code>head</code> <em>of the modified linked list</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/eg1.png" style="width: 699px; height: 124px;" />
<pre>
<strong>Input:</strong> head = [5,2,6,3,9,1,7,3,8,4]
<strong>Output:</strong> [5,6,2,3,9,1,4,8,3,7]
<strong>Explanation:</strong>
- The length of the first group is 1, which is odd, hence no reversal occurs.
- The length of the second group is 2, which is even, hence the nodes are reversed.
- The length of the third group is 3, which is odd, hence no reversal occurs.
- The length of the last group is 4, which is even, hence the nodes are reversed.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/eg2.png" style="width: 284px; height: 114px;" />
<pre>
<strong>Input:</strong> head = [1,1,0,6]
<strong>Output:</strong> [1,0,1,6]
<strong>Explanation:</strong>
- The length of the first group is 1. No reversal occurs.
- The length of the second group is 2. The nodes are reversed.
- The length of the last group is 1. No reversal occurs.
</pre>

<p><strong class="example">Example 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2074.Reverse%20Nodes%20in%20Even%20Length%20Groups/images/ex3.png" style="width: 348px; height: 114px;" />
<pre>
<strong>Input:</strong> head = [1,1,0,6,5]
<strong>Output:</strong> [1,0,1,5,6]
<strong>Explanation:</strong>
- The length of the first group is 1. No reversal occurs.
- The length of the second group is 2. The nodes are reversed.
- The length of the last group is 2. The nodes are reversed.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the list is in the range <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseEvenLengthGroups(ListNode head) {
        int n = 0;
        for (ListNode t = head; t != null; t = t.next) {
            ++n;
        }
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        int l = 1;
        for (; (1 + l) * l / 2 <= n && prev != null; ++l) {
            if (l % 2 == 0) {
                ListNode node = prev.next;
                prev.next = reverse(node, l);
            }
            for (int i = 0; i < l && prev != null; ++i) {
                prev = prev.next;
            }
        }
        int left = n - l * (l - 1) / 2;
        if (left > 0 && left % 2 == 0) {
            ListNode node = prev.next;
            prev.next = reverse(node, left);
        }
        return dummy.next;
    }

    private ListNode reverse(ListNode head, int l) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode tail = cur;
        int i = 0;
        while (cur != null && i < l) {
            ListNode t = cur.next;
            cur.next = prev;
            prev = cur;
            cur = t;
            ++i;
        }
        tail.next = cur;
        return prev;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
