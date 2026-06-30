---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2800-2899/2866.Beautiful%20Towers%20II/README_EN.md
rating: 2071
source: Weekly Contest 364 Q3
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2866. Beautiful Towers II](https://leetcode.com/problems/beautiful-towers-ii)

[Chinese Version](/solution/2800-2899/2866.Beautiful%20Towers%20II/README.md)

## Description

<!-- description:start -->

<p>You are given a <strong>0-indexed</strong> array <code>maxHeights</code> of <code>n</code> integers.</p>

<p>You are tasked with building <code>n</code> towers in the coordinate line. The <code>i<sup>th</sup></code> tower is built at coordinate <code>i</code> and has a height of <code>heights[i]</code>.</p>

<p>A configuration of towers is <strong>beautiful</strong> if the following conditions hold:</p>

<ol>
	<li><code>1 &lt;= heights[i] &lt;= maxHeights[i]</code></li>
	<li><code>heights</code> is a <strong>mountain</strong> array.</li>
</ol>

<p>Array <code>heights</code> is a <strong>mountain</strong> if there exists an index <code>i</code> such that:</p>

<ul>
	<li>For all <code>0 &lt; j &lt;= i</code>, <code>heights[j - 1] &lt;= heights[j]</code></li>
	<li>For all <code>i &lt;= k &lt; n - 1</code>, <code>heights[k + 1] &lt;= heights[k]</code></li>
</ul>

<p>Return <em>the <strong>maximum possible sum of heights</strong> of a beautiful configuration of towers</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> maxHeights = [5,3,4,1,1]
<strong>Output:</strong> 13
<strong>Explanation:</strong> One beautiful configuration with a maximum sum is heights = [5,3,3,1,1]. This configuration is beautiful since:
- 1 &lt;= heights[i] &lt;= maxHeights[i]  
- heights is a mountain of peak i = 0.
It can be shown that there exists no other beautiful configuration with a sum of heights greater than 13.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> maxHeights = [6,5,3,9,2,7]
<strong>Output:</strong> 22
<strong>Explanation:</strong> One beautiful configuration with a maximum sum is heights = [3,3,3,9,2,2]. This configuration is beautiful since:
- 1 &lt;= heights[i] &lt;= maxHeights[i]
- heights is a mountain of peak i = 3.
It can be shown that there exists no other beautiful configuration with a sum of heights greater than 22.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> maxHeights = [3,2,5,5,2,3]
<strong>Output:</strong> 18
<strong>Explanation:</strong> One beautiful configuration with a maximum sum is heights = [2,2,5,5,2,2]. This configuration is beautiful since:
- 1 &lt;= heights[i] &lt;= maxHeights[i]
- heights is a mountain of peak i = 2. 
Note that, for this configuration, i = 3 can also be considered a peak.
It can be shown that there exists no other beautiful configuration with a sum of heights greater than 18.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n == maxHeights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxHeights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming + Monotonic Stack

We define $f[i]$ to represent the height sum of the beautiful tower scheme with the last tower as the tallest tower among the first $i+1$ towers. We can get the following state transition equation:

$$
f[i]=
\begin{cases}
f[i-1]+heights[i],&\textit{if } heights[i]\geq heights[i-1]\\
heights[i]\times(i-j)+f[j],&\textit{if } heights[i]<heights[i-1]
\end{cases}
$$

Where $j$ is the index of the first tower to the left of the last tower with a height less than or equal to $heights[i]$. We can use a monotonic stack to maintain this index.

We can use a similar method to find $g[i]$, which represents the height sum of the beautiful tower scheme from right to left with the $i$th tower as the tallest tower. The final answer is the maximum value of $f[i]+g[i]-heights[i]$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $maxHeights$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        int n = maxHeights.size();
        Deque<Integer> stk = new ArrayDeque<>();
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) > x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) >= x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long[] f = new long[n];
        long[] g = new long[n];
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            if (i > 0 && x >= maxHeights.get(i - 1)) {
                f[i] = f[i - 1] + x;
            } else {
                int j = left[i];
                f[i] = 1L * x * (i - j) + (j >= 0 ? f[j] : 0);
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            if (i < n - 1 && x >= maxHeights.get(i + 1)) {
                g[i] = g[i + 1] + x;
            } else {
                int j = right[i];
                g[i] = 1L * x * (j - i) + (j < n ? g[j] : 0);
            }
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, f[i] + g[i] - maxHeights.get(i));
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
