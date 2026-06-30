---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3300-3399/3334.Find%20the%20Maximum%20Factor%20Score%20of%20Array/README_EN.md
rating: 1518
source: Weekly Contest 421 Q1
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3334. Find the Maximum Factor Score of Array](https://leetcode.com/problems/find-the-maximum-factor-score-of-array)

[Chinese Version](/solution/3300-3399/3334.Find%20the%20Maximum%20Factor%20Score%20of%20Array/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>.</p>

<p>The <strong>factor score</strong> of an array is defined as the <em>product</em> of the LCM and GCD of all elements of that array.</p>

<p>Return the <strong>maximum factor score</strong> of <code>nums</code> after removing <strong>at most</strong> one element from it.</p>

<p><strong>Note</strong> that <em>both</em> the <span data-keyword="lcm-function">LCM</span> and <span data-keyword="gcd-function">GCD</span> of a single number are the number itself, and the <em>factor score</em> of an <strong>empty</strong> array is 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [2,4,8,16]</span></p>

<p><strong>Output:</strong> <span class="example-io">64</span></p>

<p><strong>Explanation:</strong></p>

<p>On removing 2, the GCD of the rest of the elements is 4 while the LCM is 16, which gives a maximum factor score of <code>4 * 16 = 64</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Output:</strong> <span class="example-io">60</span></p>

<p><strong>Explanation:</strong></p>

<p>The maximum factor score of 60 can be obtained without removing any elements.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [3]</span></p>

<p><strong>Output:</strong> 9</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long maxScore(int[] nums) {
        int n = nums.length;
        long[] sufGcd = new long[n + 1];
        long[] sufLcm = new long[n + 1];
        sufLcm[n] = 1;
        for (int i = n - 1; i >= 0; --i) {
            sufGcd[i] = gcd(sufGcd[i + 1], nums[i]);
            sufLcm[i] = lcm(sufLcm[i + 1], nums[i]);
        }
        long ans = sufGcd[0] * sufLcm[0];
        long preGcd = 0, preLcm = 1;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, gcd(preGcd, sufGcd[i + 1]) * lcm(preLcm, sufLcm[i + 1]));
            preGcd = gcd(preGcd, nums[i]);
            preLcm = lcm(preLcm, nums[i]);
        }
        return ans;
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private long lcm(long a, long b) {
        return a / gcd(a, b) * b;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
