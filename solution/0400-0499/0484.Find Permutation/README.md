---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0400-0499/0484.Find%20Permutation/README_EN.md
tags:
    - Stack
    - Greedy
    - Array
    - String
---

<!-- problem:start -->

# [484. Find Permutation 🔒](https://leetcode.com/problems/find-permutation)

[Chinese Version](/solution/0400-0499/0484.Find%20Permutation/README.md)

## Description

<!-- description:start -->

<p>A permutation <code>perm</code> of <code>n</code>&nbsp;integers of all the integers in the range <code>[1, n]</code> can be represented as a string <code>s</code> of length <code>n - 1</code> where:</p>

<ul>
	<li><code>s[i] == &#39;I&#39;</code> if <code>perm[i] &lt; perm[i + 1]</code>, and</li>
	<li><code>s[i] == &#39;D&#39;</code> if <code>perm[i] &gt; perm[i + 1]</code>.</li>
</ul>

<p>Given a string <code>s</code>, reconstruct the lexicographically smallest permutation <code>perm</code> and return it.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;I&quot;
<strong>Output:</strong> [1,2]
<strong>Explanation:</strong> [1,2] is the only legal permutation that can represented by s, where the number 1 and 2 construct an increasing relationship.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;DI&quot;
<strong>Output:</strong> [2,1,3]
<strong>Explanation:</strong> Both [2,1,3] and [3,1,2] can be represented as &quot;DI&quot;, but since we want to find the smallest lexicographical permutation, you should return [2,1,3]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> is either <code>&#39;I&#39;</code> or <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] findPermutation(String s) {
        int n = s.length();
        int[] ans = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            ans[i] = i + 1;
        }
        int i = 0;
        while (i < n) {
            int j = i;
            while (j < n && s.charAt(j) == 'D') {
                ++j;
            }
            reverse(ans, i, j);
            i = Math.max(i + 1, j);
        }
        return ans;
    }

    private void reverse(int[] arr, int i, int j) {
        for (; i < j; ++i, --j) {
            int t = arr[i];
            arr[i] = arr[j];
            arr[j] = t;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
