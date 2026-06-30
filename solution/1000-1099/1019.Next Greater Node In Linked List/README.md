---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/README_EN.md
rating: 1570
source: Weekly Contest 130 Q3
tags:
    - Stack
    - Array
    - Linked List
    - Monotonic Stack
---

<!-- problem:start -->

# [1019. Next Greater Node In Linked List](https://leetcode.com/problems/next-greater-node-in-linked-list)

[Chinese Version](/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/README.md)

## Description

<!-- description:start -->

<p>You are given the <code>head</code> of a linked list with <code>n</code> nodes.</p>

<p>For each node in the list, find the value of the <strong>next greater node</strong>. That is, for each node, find the value of the first node that is next to it and has a <strong>strictly larger</strong> value than it.</p>

<p>Return an integer array <code>answer</code> where <code>answer[i]</code> is the value of the next greater node of the <code>i<sup>th</sup></code> node (<strong>1-indexed</strong>). If the <code>i<sup>th</sup></code> node does not have a next greater node, set <code>answer[i] = 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/images/linkedlistnext1.jpg" style="width: 304px; height: 133px;" />
<pre>
<strong>Input:</strong> head = [2,1,5]
<strong>Output:</strong> [5,5,0]
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/images/linkedlistnext2.jpg" style="width: 500px; height: 113px;" />
<pre>
<strong>Input:</strong> head = [2,7,4,3,5]
<strong>Output:</strong> [7,0,5,5,0]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the list is <code>n</code>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Monotonic Stack

The problem requires finding the next larger node for each node in the linked list, that is, finding the first node to the right of each node in the linked list that is larger than it. We first traverse the linked list and store the values in the linked list in an array $nums$. For each element in the array $nums$, we just need to find the first element to its right that is larger than it. The problem of finding the next larger element can be solved using a monotonic stack.

We traverse the array $nums$ from back to front, maintaining a stack $stk$ that is monotonically decreasing from the bottom to the top. During the traversal, if the top element of the stack is less than or equal to the current element, we loop to pop the top element of the stack until the top element of the stack is larger than the current element or the stack is empty.

If the stack is empty at this time, it means that the current element does not have a next larger element, otherwise, the next larger element of the current element is the top element of the stack, and we update the answer array $ans$. Then we push the current element into the stack and continue the traversal.

After the traversal is over, we return the answer array $ans$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the linked list.

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
    public int[] nextLargerNodes(ListNode head) {
        List<Integer> nums = new ArrayList<>();
        for (; head != null; head = head.next) {
            nums.add(head.val);
        }
        Deque<Integer> stk = new ArrayDeque<>();
        int n = nums.size();
        int[] ans = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && stk.peek() <= nums.get(i)) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                ans[i] = stk.peek();
            }
            stk.push(nums.get(i));
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
