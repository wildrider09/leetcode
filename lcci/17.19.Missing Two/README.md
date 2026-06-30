---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.19.Missing%20Two/README_EN.md
---

<!-- problem:start -->

# [17.19. Missing Two](https://leetcode.cn/problems/missing-two-lcci)

[Chinese Version](/lcci/17.19.Missing%20Two/README.md)

## Description

<!-- description:start -->

<p>You are given an array with all the numbers from 1 to N appearing exactly once, except for two number that is missing. How can you find the missing number in O(N) time and 0(1) space?</p>

<p>You can return the missing numbers in any order.</p>

<p><strong>Example 1:</strong></p>

<pre>

<strong>Input:</strong> [1]

<strong>Output: </strong>[2,3]</pre>

<p><strong>Example 2:</strong></p>

<pre>

<strong>Input:</strong> [2,3]

<strong>Output: </strong>[1,4]</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>nums.length &lt;=&nbsp;30000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] missingTwo(int[] nums) {
        int n = nums.length + 2;
        int xor = 0;
        for (int v : nums) {
            xor ^= v;
        }
        for (int i = 1; i <= n; ++i) {
            xor ^= i;
        }
        int diff = xor & (-xor);
        int a = 0;
        for (int v : nums) {
            if ((v & diff) != 0) {
                a ^= v;
            }
        }
        for (int i = 1; i <= n; ++i) {
            if ((i & diff) != 0) {
                a ^= i;
            }
        }
        int b = xor ^ a;
        return new int[] {a, b};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
