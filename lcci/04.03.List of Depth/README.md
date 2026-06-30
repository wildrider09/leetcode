---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/04.03.List%20of%20Depth/README_EN.md
---

<!-- problem:start -->

# [04.03. List of Depth](https://leetcode.cn/problems/list-of-depth-lcci)

[Chinese Version](/lcci/04.03.List%20of%20Depth/README.md)

## Description

<!-- description:start -->

<p>Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you&#39;ll have D linked lists). Return a array containing all the linked lists.</p>

<p>&nbsp;</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>[1,2,3,4,5,null,7,8]

        1

       /  \

      2    3

     / \    \

    4   5    7

   /

  8

<strong>Output: </strong>[[1],[2,3],[4,5,7],[8]]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: BFS Level Order Traversal

We can use the BFS level order traversal method. For each level, we store the values of the current level's nodes into a list, and then add the list to the result array.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the number of nodes in the binary tree.

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode[] listOfDepth(TreeNode tree) {
        List<ListNode> ans = new ArrayList<>();
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(tree);
        while (!q.isEmpty()) {
            ListNode dummy = new ListNode(0);
            ListNode cur = dummy;
            for (int k = q.size(); k > 0; --k) {
                TreeNode node = q.poll();
                cur.next = new ListNode(node.val);
                cur = cur.next;
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            ans.add(dummy.next);
        }
        return ans.toArray(new ListNode[0]);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
