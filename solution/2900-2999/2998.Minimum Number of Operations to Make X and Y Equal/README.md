---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2900-2999/2998.Minimum%20Number%20of%20Operations%20to%20Make%20X%20and%20Y%20Equal/README_EN.md
rating: 1794
source: Biweekly Contest 121 Q3
tags:
    - Breadth-First Search
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [2998. Minimum Number of Operations to Make X and Y Equal](https://leetcode.com/problems/minimum-number-of-operations-to-make-x-and-y-equal)

[Chinese Version](/solution/2900-2999/2998.Minimum%20Number%20of%20Operations%20to%20Make%20X%20and%20Y%20Equal/README.md)

## Description

<!-- description:start -->

<p>You are given two positive integers <code>x</code> and <code>y</code>.</p>

<p>In one operation, you can do one of the four following operations:</p>

<ol>
	<li>Divide <code>x</code> by <code>11</code> if <code>x</code> is a multiple of <code>11</code>.</li>
	<li>Divide <code>x</code> by <code>5</code> if <code>x</code> is a multiple of <code>5</code>.</li>
	<li>Decrement <code>x</code> by <code>1</code>.</li>
	<li>Increment <code>x</code> by <code>1</code>.</li>
</ol>

<p>Return <em>the <strong>minimum</strong> number of operations required to make </em> <code>x</code> <i>and</i> <code>y</code> equal.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> x = 26, y = 1
<strong>Output:</strong> 3
<strong>Explanation:</strong> We can make 26 equal to 1 by applying the following operations: 
1. Decrement x by 1
2. Divide x by 5
3. Divide x by 5
It can be shown that 3 is the minimum number of operations required to make 26 equal to 1.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> x = 54, y = 2
<strong>Output:</strong> 4
<strong>Explanation:</strong> We can make 54 equal to 2 by applying the following operations: 
1. Increment x by 1
2. Divide x by 11 
3. Divide x by 5
4. Increment x by 1
It can be shown that 4 is the minimum number of operations required to make 54 equal to 2.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> x = 25, y = 30
<strong>Output:</strong> 5
<strong>Explanation:</strong> We can make 25 equal to 30 by applying the following operations: 
1. Increment x by 1
2. Increment x by 1
3. Increment x by 1
4. Increment x by 1
5. Increment x by 1
It can be shown that 5 is the minimum number of operations required to make 25 equal to 30.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= x, y &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Map<Integer, Integer> f = new HashMap<>();
    private int y;

    public int minimumOperationsToMakeEqual(int x, int y) {
        this.y = y;
        return dfs(x);
    }

    private int dfs(int x) {
        if (y >= x) {
            return y - x;
        }
        if (f.containsKey(x)) {
            return f.get(x);
        }
        int ans = x - y;
        int a = x % 5 + 1 + dfs(x / 5);
        int b = 5 - x % 5 + 1 + dfs(x / 5 + 1);
        int c = x % 11 + 1 + dfs(x / 11);
        int d = 11 - x % 11 + 1 + dfs(x / 11 + 1);
        ans = Math.min(ans, Math.min(a, Math.min(b, Math.min(c, d))));
        f.put(x, ans);
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
