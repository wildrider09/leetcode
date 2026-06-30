---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0500-0599/0536.Construct%20Binary%20Tree%20from%20String/README_EN.md
tags:
    - Stack
    - Tree
    - Depth-First Search
    - String
    - Binary Tree
---

<!-- problem:start -->

# [536. Construct Binary Tree from String 🔒](https://leetcode.com/problems/construct-binary-tree-from-string)

[Chinese Version](/solution/0500-0599/0536.Construct%20Binary%20Tree%20from%20String/README.md)

## Description

<!-- description:start -->

<p>You need to construct a binary tree from a string consisting of parenthesis and integers.</p>

<p>The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root&#39;s value and a pair of parenthesis contains a child binary tree with the same structure.</p>

<p>You always start to construct the <b>left</b> child node of the parent first if it exists.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0536.Construct%20Binary%20Tree%20from%20String/images/butree.jpg" style="width: 382px; height: 322px;" />
<pre>
<strong>Input:</strong> s = &quot;4(2(3)(1))(6(5))&quot;
<strong>Output:</strong> [4,2,6,3,1,5]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;4(2(3)(1))(6(5)(7))&quot;
<strong>Output:</strong> [4,2,6,3,1,5,7]
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;-4(2(3)(1))(6(5)(7))&quot;
<strong>Output:</strong> [-4,2,6,3,1,5,7]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> consists of digits, <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>, and <code>&#39;-&#39;</code> only.</li>
	<li>All numbers in the tree have value <strong>at most</strong> than <code>2<sup>30</sup></code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode str2tree(String s) {
        return dfs(s);
    }

    private TreeNode dfs(String s) {
        if ("".equals(s)) {
            return null;
        }
        int p = s.indexOf("(");
        if (p == -1) {
            return new TreeNode(Integer.parseInt(s));
        }
        TreeNode root = new TreeNode(Integer.parseInt(s.substring(0, p)));
        int start = p;
        int cnt = 0;
        for (int i = p; i < s.length(); ++i) {
            if (s.charAt(i) == '(') {
                ++cnt;
            } else if (s.charAt(i) == ')') {
                --cnt;
            }
            if (cnt == 0) {
                if (start == p) {
                    root.left = dfs(s.substring(start + 1, i));
                    start = i + 1;
                } else {
                    root.right = dfs(s.substring(start + 1, i));
                }
            }
        }
        return root;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
