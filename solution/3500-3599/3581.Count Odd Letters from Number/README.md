---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3500-3599/3581.Count%20Odd%20Letters%20from%20Number/README_EN.md
tags:
    - Hash Table
    - String
    - Counting
    - Simulation
---

<!-- problem:start -->

# [3581. Count Odd Letters from Number 🔒](https://leetcode.com/problems/count-odd-letters-from-number)

[Chinese Version](/solution/3500-3599/3581.Count%20Odd%20Letters%20from%20Number/README.md)

## Description

<!-- description:start -->

<p>You are given an integer <code>n</code> perform the following steps:</p>

<ul>
	<li>Convert each digit of <code>n</code> into its <em>lowercase English word</em> (e.g., 4 &rarr; &quot;four&quot;, 1 &rarr; &quot;one&quot;).</li>
	<li><strong>Concatenate</strong> those words in the <strong>original digit order</strong> to form a string <code>s</code>.</li>
</ul>

<p>Return the number of <strong>distinct</strong> characters in <code>s</code> that appear an <strong>odd</strong> number of times.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 41</span></p>

<p><strong>Output:</strong> <span class="example-io">5</span></p>

<p><strong>Explanation:</strong></p>

<p>41 &rarr; <code>&quot;fourone&quot;</code></p>

<p>Characters with odd frequencies: <code>&#39;f&#39;</code>, <code>&#39;u&#39;</code>, <code>&#39;r&#39;</code>, <code>&#39;n&#39;</code>, <code>&#39;e&#39;</code>. Thus, the answer is 5.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 20</span></p>

<p><strong>Output:</strong> <span class="example-io">5</span></p>

<p><strong>Explanation:</strong></p>

<p>20 &rarr; <code>&quot;twozero&quot;</code></p>

<p>Characters with odd frequencies: <code>&#39;t&#39;</code>, <code>&#39;w&#39;</code>, <code>&#39;z&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;r&#39;</code>. Thus, the answer is 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation + Bit Manipulation

We can convert each number into its corresponding English word, then count the frequency of each letter. Since the number of letters is limited, we can use an integer $\textit{mask}$ to represent the occurrence of each letter. Specifically, we can map each letter to a binary bit of the integer. If a letter appears an odd number of times, the corresponding binary bit is 1; otherwise, it's 0. Finally, we only need to count the number of bits that are 1 in $\textit{mask}$, which is the answer.

The time complexity is $O(\log n)$, where $n$ is the input integer. And the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final Map<Integer, String> d = new HashMap<>();
    static {
        d.put(0, "zero");
        d.put(1, "one");
        d.put(2, "two");
        d.put(3, "three");
        d.put(4, "four");
        d.put(5, "five");
        d.put(6, "six");
        d.put(7, "seven");
        d.put(8, "eight");
        d.put(9, "nine");
    }

    public int countOddLetters(int n) {
        int mask = 0;
        while (n > 0) {
            int x = n % 10;
            n /= 10;
            for (char c : d.get(x).toCharArray()) {
                mask ^= 1 << (c - 'a');
            }
        }
        return Integer.bitCount(mask);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
