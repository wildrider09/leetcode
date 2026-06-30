---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3100-3199/3153.Sum%20of%20Digit%20Differences%20of%20All%20Pairs/README_EN.md
rating: 1645
source: Weekly Contest 398 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [3153. Sum of Digit Differences of All Pairs](https://leetcode.com/problems/sum-of-digit-differences-of-all-pairs)

[Chinese Version](/solution/3100-3199/3153.Sum%20of%20Digit%20Differences%20of%20All%20Pairs/README.md)

## Description

<!-- description:start -->

<p>You are given an array <code>nums</code> consisting of <strong>positive</strong> integers where all integers have the <strong>same</strong> number of digits.</p>

<p>The <strong>digit difference</strong> between two integers is the <em>count</em> of different digits that are in the <strong>same</strong> position in the two integers.</p>

<p>Return the <strong>sum</strong> of the <strong>digit differences</strong> between <strong>all</strong> pairs of integers in <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [13,23,12]</span></p>

<p><strong>Output:</strong> 4</p>

<p><strong>Explanation:</strong><br />
We have the following:<br />
- The digit difference between <strong>1</strong>3 and <strong>2</strong>3 is 1.<br />
- The digit difference between 1<strong>3</strong> and 1<strong>2</strong> is 1.<br />
- The digit difference between <strong>23</strong> and <strong>12</strong> is 2.<br />
So the total sum of digit differences between all pairs of integers is <code>1 + 1 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [10,10,10,10]</span></p>

<p><strong>Output:</strong> <span class="example-io">0</span></p>

<p><strong>Explanation:</strong><br />
All the integers in the array are the same. So the total sum of digit differences between all pairs of integers will be 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt; 10<sup>9</sup></code></li>
	<li>All integers in <code>nums</code> have the same number of digits.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Counting

First, we get the number of digits $m$ in the array. Then for each digit, we count the occurrence of each number at this digit in the array `nums`, denoted as `cnt`. Therefore, the sum of the digit differences of all number pairs at this digit is:

$$
\sum_{v \in \textit{cnt}} v \times (n - v)
$$

where $n$ is the length of the array. We add up the digit differences of all digits and divide by $2$ to get the answer.

The time complexity is $O(n \times m)$, and the space complexity is $O(C)$, where $n$ and $m$ are the length of the array and the number of digits in the numbers, respectively; and $C$ is a constant, in this problem $C = 10$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public long sumDigitDifferences(int[] nums) {
        int n = nums.length;
        int m = (int) Math.floor(Math.log10(nums[0])) + 1;
        int[] cnt = new int[10];
        long ans = 0;
        for (int k = 0; k < m; ++k) {
            Arrays.fill(cnt, 0);
            for (int i = 0; i < n; ++i) {
                ++cnt[nums[i] % 10];
                nums[i] /= 10;
            }
            for (int i = 0; i < 10; ++i) {
                ans += 1L * cnt[i] * (n - cnt[i]);
            }
        }
        return ans / 2;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
