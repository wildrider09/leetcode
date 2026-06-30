---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0800-0899/0873.Length%20of%20Longest%20Fibonacci%20Subsequence/README_EN.md
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [873. Length of Longest Fibonacci Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence)

[Chinese Version](/solution/0800-0899/0873.Length%20of%20Longest%20Fibonacci%20Subsequence/README.md)

## Description

<!-- description:start -->

<p>A sequence <code>x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>n</sub></code> is <em>Fibonacci-like</em> if:</p>

<ul>
	<li><code>n &gt;= 3</code></li>
	<li><code>x<sub>i</sub> + x<sub>i+1</sub> == x<sub>i+2</sub></code> for all <code>i + 2 &lt;= n</code></li>
</ul>

<p>Given a <b>strictly increasing</b> array <code>arr</code> of positive integers forming a sequence, return <em>the <strong>length</strong> of the longest Fibonacci-like subsequence of</em> <code>arr</code>. If one does not exist, return <code>0</code>.</p>

<p>A <strong>subsequence</strong> is derived from another sequence <code>arr</code> by deleting any number of elements (including none) from <code>arr</code>, without changing the order of the remaining elements. For example, <code>[3, 5, 8]</code> is a subsequence of <code>[3, 4, 5, 6, 7, 8]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5,6,7,8]
<strong>Output:</strong> 5
<strong>Explanation:</strong> The longest subsequence that is fibonacci-like: [1,2,3,5,8].</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,3,7,11,12,14,18]
<strong>Output:</strong> 3
<strong>Explanation</strong>:<strong> </strong>The longest subsequence that is fibonacci-like: [1,11,12], [3,11,14] or [7,11,18].</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt; arr[i + 1] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i][j]$ as the length of the longest Fibonacci-like subsequence, with $\textit{arr}[i]$ as the last element and $\textit{arr}[j]$ as the second to last element. Initially, for any $i \in [0, n)$ and $j \in [0, i)$, we have $f[i][j] = 2$. All other elements are $0$.

We use a hash table $d$ to record the indices of each element in the array $\textit{arr}$.

Then, we can enumerate $\textit{arr}[i]$ and $\textit{arr}[j]$, where $i \in [2, n)$ and $j \in [1, i)$. Suppose the currently enumerated elements are $\textit{arr}[i]$ and $\textit{arr}[j]$, we can obtain $\textit{arr}[i] - \textit{arr}[j]$, denoted as $t$. If $t$ is in the array $\textit{arr}$, and the index $k$ of $t$ satisfies $k < j$, then we can get a Fibonacci-like subsequence with $\textit{arr}[j]$ and $\textit{arr}[i]$ as the last two elements, and its length is $f[i][j] = \max(f[i][j], f[j][k] + 1)$. We can continuously update the value of $f[i][j]$ in this way, and then update the answer.

After the enumeration ends, return the answer.

The time complexity is $O(n^2)$, and the space complexity is $O(n^2)$, where $n$ is the length of the array $\textit{arr}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int lenLongestFibSubseq(int[] arr) {
        int n = arr.length;
        int[][] f = new int[n][n];
        Map<Integer, Integer> d = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            d.put(arr[i], i);
            for (int j = 0; j < i; ++j) {
                f[i][j] = 2;
            }
        }
        int ans = 0;
        for (int i = 2; i < n; ++i) {
            for (int j = 1; j < i; ++j) {
                int t = arr[i] - arr[j];
                Integer k = d.get(t);
                if (k != null && k < j) {
                    f[i][j] = Math.max(f[i][j], f[j][k] + 1);
                    ans = Math.max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
