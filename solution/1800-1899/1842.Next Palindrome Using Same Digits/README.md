---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1800-1899/1842.Next%20Palindrome%20Using%20Same%20Digits/README_EN.md
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1842. Next Palindrome Using Same Digits 🔒](https://leetcode.com/problems/next-palindrome-using-same-digits)

[Chinese Version](/solution/1800-1899/1842.Next%20Palindrome%20Using%20Same%20Digits/README.md)

## Description

<!-- description:start -->

<p>You are given a numeric string <code>num</code>, representing a very large <strong>palindrome</strong>.</p>

<p>Return<em> the <strong>smallest palindrome larger than </strong></em><code>num</code><em> that can be created by rearranging its digits. If no such palindrome exists, return an empty string </em><code>&quot;&quot;</code>.</p>

<p>A <strong>palindrome</strong> is a number that reads the same backward as forward.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;1221&quot;
<strong>Output:</strong> &quot;2112&quot;
<strong>Explanation:</strong>&nbsp;The next palindrome larger than &quot;1221&quot; is &quot;2112&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;32123&quot;
<strong>Output:</strong> &quot;&quot;
<strong>Explanation:</strong>&nbsp;No palindromes larger than &quot;32123&quot; can be made by rearranging the digits.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;45544554&quot;
<strong>Output:</strong> &quot;54455445&quot;
<strong>Explanation:</strong> The next palindrome larger than &quot;45544554&quot; is &quot;54455445&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> is a <strong>palindrome</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Find the Next Permutation of the First Half

According to the problem description, we only need to find the next permutation of the first half of the string, then traverse the first half and symmetrically assign values to the second half.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the string.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String nextPalindrome(String num) {
        char[] nums = num.toCharArray();
        if (!nextPermutation(nums)) {
            return "";
        }
        int n = nums.length;
        for (int i = 0; i < n / 2; ++i) {
            nums[n - 1 - i] = nums[i];
        }
        return String.valueOf(nums);
    }

    private boolean nextPermutation(char[] nums) {
        int n = nums.length / 2;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            --i;
        }
        if (i < 0) {
            return false;
        }
        int j = n - 1;
        while (j >= 0 && nums[i] >= nums[j]) {
            --j;
        }
        swap(nums, i++, j);
        for (j = n - 1; i < j; ++i, --j) {
            swap(nums, i, j);
        }
        return true;
    }

    private void swap(char[] nums, int i, int j) {
        char t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
