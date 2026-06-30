---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3000-3099/3073.Maximum%20Increasing%20Triplet%20Value/README_EN.md
tags:
    - Array
    - Ordered Set
---

<!-- problem:start -->

# [3073. Maximum Increasing Triplet Value 🔒](https://leetcode.com/problems/maximum-increasing-triplet-value)

[Chinese Version](/solution/3000-3099/3073.Maximum%20Increasing%20Triplet%20Value/README.md)

## Description

<!-- description:start -->

<p>Given an array <code>nums</code>, return <em>the <strong>maximum value</strong> of a triplet</em> <code>(i, j, k)</code> <em>such that</em> <code>i &lt; j &lt; k</code> <em>and</em> <code>nums[i] &lt; nums[j] &lt; nums[k]</code>.</p>

<p>The <strong>value</strong> of a triplet <code>(i, j, k)</code> is <code>nums[i] - nums[j] + nums[k]</code>.</p>

<div id="gtx-trans" style="position: absolute; left: 274px; top: 102px;">
<div class="gtx-trans-icon"> </div>
</div>

<p>&nbsp;</p>
<p><strong class="example">Example 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Input: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [5,6,9] </span></p>

<p><strong>Output: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">8 </span></p>

<p><strong>Explanation: </strong> We only have one choice for an increasing triplet and that is choosing all three elements. The value of this triplet would be <code>5 - 6 + 9 = 8</code>.</p>
</div>

<p><strong class="example">Example 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Input:</strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [1,5,3,6] </span></p>

<p><strong>Output:</strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 4 </span></p>

<p><strong>Explanation: </strong> There are only two increasing triplets:</p>

<p><code>(0, 1, 3)</code>: The value of this triplet is <code>nums[0] - nums[1] + nums[3] = 1 - 5 + 6 = 2</code>.</p>

<p><code>(0, 2, 3)</code>: The value of this triplet is <code>nums[0] - nums[2] + nums[3] = 1 - 3 + 6 = 4</code>.</p>

<p>Thus the answer would be <code>4</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>The input is generated such that at least one triplet meets the given condition.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Suffix Maximum + Ordered Set

We can consider enumerating $nums[j]$. Then, we need to find the largest $nums[i]$ on the left of $j$ such that $nums[i] < nums[j]$, and find the largest $nums[k]$ on the right of $j$ such that $nums[k] > nums[j]$.

Therefore, we can preprocess an array $right$, where $right[i]$ represents the maximum value to the right of $nums[i]$. Then, we can use an ordered set to maintain the values on the left of $nums[j]$, so that we can find the largest $nums[i]$ less than $nums[j]$ in $O(\log n)$ time.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$, where $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maximumTripletValue(int[] nums) {
        int n = nums.length;
        int[] right = new int[n];
        right[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            right[i] = Math.max(nums[i], right[i + 1]);
        }
        TreeSet<Integer> ts = new TreeSet<>();
        ts.add(nums[0]);
        int ans = 0;
        for (int j = 1; j < n - 1; ++j) {
            if (right[j + 1] > nums[j]) {
                Integer it = ts.lower(nums[j]);
                if (it != null) {
                    ans = Math.max(ans, it - nums[j] + right[j + 1]);
                }
            }
            ts.add(nums[j]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
