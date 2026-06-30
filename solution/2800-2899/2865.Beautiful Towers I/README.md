---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2800-2899/2865.Beautiful%20Towers%20I/README_EN.md
rating: 1519
source: Weekly Contest 364 Q2
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2865. Beautiful Towers I](https://leetcode.com/problems/beautiful-towers-i)

[Chinese Version](/solution/2800-2899/2865.Beautiful%20Towers%20I/README.md)

## Description

<!-- description:start -->

<p>You are given an array <code>heights</code> of <code>n</code> integers representing the number of bricks in <code>n</code> consecutive towers. Your task is to remove some bricks to form a <strong>mountain-shaped</strong> tower arrangement. In this arrangement, the tower heights are non-decreasing, reaching a maximum peak value with one or multiple consecutive towers and then non-increasing.</p>

<p>Return the <strong>maximum possible sum</strong> of heights of a mountain-shaped tower arrangement.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">heights = [5,3,4,1,1]</span></p>

<p><strong>Output:</strong> <span class="example-io">13</span></p>

<p><strong>Explanation:</strong></p>

<p>We remove some bricks to make <code>heights =&nbsp;[5,3,3,1,1]</code>, the peak is at index 0.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">heights = [6,5,3,9,2,7]</span></p>

<p><strong>Output:</strong> <span class="example-io">22</span></p>

<p><strong>Explanation:</strong></p>

<p>We remove some bricks to make <code>heights =&nbsp;[3,3,3,9,2,2]</code>, the peak is at index 3.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">heights = [3,2,5,5,2,3]</span></p>

<p><strong>Output:</strong> <span class="example-io">18</span></p>

<p><strong>Explanation:</strong></p>

<p>We remove some bricks to make <code>heights = [2,2,5,5,2,2]</code>, the peak is at index 2 or 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n == heights.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We can enumerate each tower as the tallest tower, each time expanding to the left and right, calculating the height of each other position, and then accumulating to get the height sum $t$. The maximum of all height sums is the answer.

The time complexity is $O(n^2)$, and the space complexity is $O(1)$. Here, $n$ is the length of the array $maxHeights$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        long ans = 0;
        int n = maxHeights.size();
        for (int i = 0; i < n; ++i) {
            int y = maxHeights.get(i);
            long t = y;
            for (int j = i - 1; j >= 0; --j) {
                y = Math.min(y, maxHeights.get(j));
                t += y;
            }
            y = maxHeights.get(i);
            for (int j = i + 1; j < n; ++j) {
                y = Math.min(y, maxHeights.get(j));
                t += y;
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Dynamic Programming + Monotonic Stack

Solution 1 is sufficient to pass this problem, but the time complexity is relatively high. We can use "Dynamic Programming + Monotonic Stack" to optimize the enumeration process.

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
