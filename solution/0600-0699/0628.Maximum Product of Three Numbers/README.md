---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0628.Maximum%20Product%20of%20Three%20Numbers/README_EN.md
tags:
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers)

[Chinese Version](/solution/0600-0699/0628.Maximum%20Product%20of%20Three%20Numbers/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code>, <em>find three numbers whose product is maximum and return the maximum product</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 6
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,3,4]
<strong>Output:</strong> 24
</pre><p><strong class="example">Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [-1,-2,-3]
<strong>Output:</strong> -6
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;=&nbsp;10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + Case Analysis

First, we sort the array $\textit{nums}$, and then discuss two cases:

- If $\textit{nums}$ contains all non-negative or all non-positive numbers, the answer is the product of the last three numbers, i.e., $\textit{nums}[n-1] \times \textit{nums}[n-2] \times \textit{nums}[n-3]$;
- If $\textit{nums}$ contains both positive and negative numbers, the answer could be the product of the two smallest negative numbers and the largest positive number, i.e., $\textit{nums}[n-1] \times \textit{nums}[0] \times \textit{nums}[1]$, or the product of the last three numbers, i.e., $\textit{nums}[n-1] \times \textit{nums}[n-2] \times \textit{nums}[n-3]$.

Finally, return the maximum of the two cases.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of the array $\textit{nums}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumProduct(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int a = nums[n - 1] * nums[n - 2] * nums[n - 3];
        int b = nums[n - 1] * nums[0] * nums[1];
        return Math.max(a, b);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Single Pass

We can avoid sorting the array by maintaining five variables: $\textit{mi1}$ and $\textit{mi2}$ represent the two smallest numbers in the array, while $\textit{mx1}$, $\textit{mx2}$, and $\textit{mx3}$ represent the three largest numbers in the array.

Finally, return $\max(\textit{mi1} \times \textit{mi2} \times \textit{mx1}, \textit{mx1} \times \textit{mx2} \times \textit{mx3})$.

The time complexity is $O(n)$, where $n$ is the length of the array. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumProduct(int[] nums) {
        final int inf = 1 << 30;
        int mi1 = inf, mi2 = inf;
        int mx1 = -inf, mx2 = -inf, mx3 = -inf;
        for (int x : nums) {
            if (x < mi1) {
                mi2 = mi1;
                mi1 = x;
            } else if (x < mi2) {
                mi2 = x;
            }
            if (x > mx1) {
                mx3 = mx2;
                mx2 = mx1;
                mx1 = x;
            } else if (x > mx2) {
                mx3 = mx2;
                mx2 = x;
            } else if (x > mx3) {
                mx3 = x;
            }
        }
        return Math.max(mi1 * mi2 * mx1, mx1 * mx2 * mx3);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
