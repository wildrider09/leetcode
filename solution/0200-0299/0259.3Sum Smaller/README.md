---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0200-0299/0259.3Sum%20Smaller/README_EN.md
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [259. 3Sum Smaller 🔒](https://leetcode.com/problems/3sum-smaller)

[Chinese Version](/solution/0200-0299/0259.3Sum%20Smaller/README.md)

## Description

<!-- description:start -->

<p>Given an array of <code>n</code> integers <code>nums</code> and an integer&nbsp;<code>target</code>, find the number of index triplets <code>i</code>, <code>j</code>, <code>k</code> with <code>0 &lt;= i &lt; j &lt; k &lt; n</code> that satisfy the condition <code>nums[i] + nums[j] + nums[k] &lt; target</code>.</p>
<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [-2,0,1,3], target = 2
<strong>Output:</strong> 2
<strong>Explanation:</strong> Because there are two triplets which sums are less than 2:
[-2,0,1]
[-2,0,3]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [], target = 0
<strong>Output:</strong> 0
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [0], target = 0
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>0 &lt;= n &lt;= 3500</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>-100 &lt;= target &lt;= 100</code></li>
	<li>The input is generated such that the answer is less than or equal to 10<sup>9</sup>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + Two Pointers + Enumeration

Since the order of elements does not affect the result, we can sort the array first and then use the two-pointer method to solve this problem.

First, we sort the array and then enumerate the first element $\textit{nums}[i]$. Within the range $\textit{nums}[i+1:n-1]$, we use two pointers pointing to $\textit{nums}[j]$ and $\textit{nums}[k]$, where $j$ is the next element of $\textit{nums}[i]$ and $k$ is the last element of the array.

- If $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k] < \textit{target}$, then for any element $j \lt k' \leq k$, we have $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k'] < \textit{target}$. There are $k - j$ such $k'$, and we add $k - j$ to the answer. Next, move $j$ one position to the right and continue to find the next $k$ that meets the condition until $j \geq k$.
- If $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k] \geq \textit{target}$, then for any element $j \leq j' \lt k$, it is impossible to make $\textit{nums}[i] + \textit{nums}[j'] + \textit{nums}[k] < \textit{target}$. Therefore, we move $k$ one position to the left and continue to find the next $k$ that meets the condition until $j \geq k$.

After enumerating all $i$, we get the number of triplets that meet the condition.

The time complexity is $O(n^2)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of the array $\textit{nums}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = 0, n = nums.length;
        for (int i = 0; i + 2 < n; ++i) {
            int j = i + 1, k = n - 1;
            while (j < k) {
                int x = nums[i] + nums[j] + nums[k];
                if (x < target) {
                    ans += k - j;
                    ++j;
                } else {
                    --k;
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
