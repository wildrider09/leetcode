---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0000-0099/0085.Maximal%20Rectangle/README_EN.md
tags:
    - Stack
    - Array
    - Dynamic Programming
    - Matrix
    - Monotonic Stack
---

<!-- problem:start -->

# [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle)

[Chinese Version](/solution/0000-0099/0085.Maximal%20Rectangle/README.md)

## Description

<!-- description:start -->

<p>Given a <code>rows x cols</code>&nbsp;binary <code>matrix</code> filled with <code>0</code>&#39;s and <code>1</code>&#39;s, find the largest rectangle containing only <code>1</code>&#39;s and return <em>its area</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0000-0099/0085.Maximal%20Rectangle/images/maximal.jpg" style="width: 402px; height: 322px;" />
<pre>
<strong>Input:</strong> matrix = [[&quot;1&quot;,&quot;0&quot;,&quot;1&quot;,&quot;0&quot;,&quot;0&quot;],[&quot;1&quot;,&quot;0&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;],[&quot;1&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;],[&quot;1&quot;,&quot;0&quot;,&quot;0&quot;,&quot;1&quot;,&quot;0&quot;]]
<strong>Output:</strong> 6
<strong>Explanation:</strong> The maximal rectangle is shown in the above picture.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> matrix = [[&quot;0&quot;]]
<strong>Output:</strong> 0
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> matrix = [[&quot;1&quot;]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>rows == matrix.length</code></li>
	<li><code>cols == matrix[i].length</code></li>
	<li><code>1 &lt;= rows, cols &lt;= 200</code></li>
	<li><code>matrix[i][j]</code> is <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Monotonic Stack

We can treat each row as the base of a histogram and calculate the maximum area of the histogram for each row.

Specifically, we maintain an array $\textit{heights}$ with the same length as the number of columns in the matrix, where $\textit{heights}[j]$ represents the height of the column at the $j$-th position with the current row as the base. For each row, we iterate through each column:

- If the current element is '1', increment $\textit{heights}[j]$ by $1$.
- If the current element is '0', set $\textit{heights}[j]$ to $0$.

Then, we use the monotonic stack algorithm to calculate the maximum rectangle area of the current histogram and update the answer.

The specific steps of the monotonic stack are as follows:

1. Initialize an empty stack $\textit{stk}$ to store the indices of the columns.
2. Initialize two arrays $\textit{left}$ and $\textit{right}$, representing the index of the first column to the left and right of each column that is shorter than the current column.
3. Iterate through the heights array $\textit{heights}$, first calculating the index of the first column to the left of each column that is shorter than the current column, and store it in $\textit{left}$.
4. Then iterate through the heights array $\textit{heights}$ in reverse order, calculating the index of the first column to the right of each column that is shorter than the current column, and store it in $\textit{right}$.
5. Finally, calculate the maximum rectangle area for each column and update the answer.

The time complexity is $O(m \times n)$, where $m$ is the number of rows in $matrix$ and $n$ is the number of columns in $matrix$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int n = matrix[0].length;
        int[] heights = new int[n];
        int ans = 0;
        for (var row : matrix) {
            for (int j = 0; j < n; ++j) {
                if (row[j] == '1') {
                    heights[j] += 1;
                } else {
                    heights[j] = 0;
                }
            }
            ans = Math.max(ans, largestRectangleArea(heights));
        }
        return ans;
    }

    private int largestRectangleArea(int[] heights) {
        int res = 0, n = heights.length;
        Deque<Integer> stk = new ArrayDeque<>();
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && heights[stk.peek()] >= heights[i]) {
                right[stk.pop()] = i;
            }
            left[i] = stk.isEmpty() ? -1 : stk.peek();
            stk.push(i);
        }
        for (int i = 0; i < n; ++i) {
            res = Math.max(res, heights[i] * (right[i] - left[i] - 1));
        }
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
