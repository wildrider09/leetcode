---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0800-0899/0870.Advantage%20Shuffle/README_EN.md
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [870. Advantage Shuffle](https://leetcode.com/problems/advantage-shuffle)

[Chinese Version](/solution/0800-0899/0870.Advantage%20Shuffle/README.md)

## Description

<!-- description:start -->

<p>You are given two integer arrays <code>nums1</code> and <code>nums2</code> both of the same length. The <strong>advantage</strong> of <code>nums1</code> with respect to <code>nums2</code> is the number of indices <code>i</code> for which <code>nums1[i] &gt; nums2[i]</code>.</p>

<p>Return <em>any permutation of </em><code>nums1</code><em> that maximizes its <strong>advantage</strong> with respect to </em><code>nums2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> nums1 = [2,7,11,15], nums2 = [1,10,4,11]
<strong>Output:</strong> [2,11,7,15]
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> nums1 = [12,24,8,32], nums2 = [13,25,32,11]
<strong>Output:</strong> [24,32,8,12]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2.length == nums1.length</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[][] t = new int[n][2];
        for (int i = 0; i < n; ++i) {
            t[i] = new int[] {nums2[i], i};
        }
        Arrays.sort(t, (a, b) -> a[0] - b[0]);
        Arrays.sort(nums1);
        int[] ans = new int[n];
        int i = 0, j = n - 1;
        for (int v : nums1) {
            if (v <= t[i][0]) {
                ans[t[j--][1]] = v;
            } else {
                ans[t[i++][1]] = v;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
