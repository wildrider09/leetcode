---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3700-3799/3704.Count%20No-Zero%20Pairs%20That%20Sum%20to%20N/README_EN.md
rating: 2419
source: Weekly Contest 470 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3704. Count No-Zero Pairs That Sum to N](https://leetcode.com/problems/count-no-zero-pairs-that-sum-to-n)

[Chinese Version](/solution/3700-3799/3704.Count%20No-Zero%20Pairs%20That%20Sum%20to%20N/README.md)

## Description

<!-- description:start -->

<p>A <strong>no-zero</strong> integer is a <strong>positive</strong> integer that <strong>does not contain the digit</strong> 0 in its decimal representation.</p>

<p>Given an integer <code>n</code>, count the number of pairs <code>(a, b)</code> where:</p>

<ul>
	<li><code>a</code> and <code>b</code> are <strong>no-zero</strong> integers.</li>
	<li><code>a + b = n</code></li>
</ul>

<p>Return an integer denoting the number of such pairs.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">1</span></p>

<p><strong>Explanation:</strong></p>

<p>The only pair is <code>(1, 1)</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>The pairs are <code>(1, 2)</code> and <code>(2, 1)</code>.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 11</span></p>

<p><strong>Output:</strong> <span class="example-io">8</span></p>

<p><strong>Explanation:</strong></p>

<p>The pairs are <code>(2, 9)</code>, <code>(3, 8)</code>, <code>(4, 7)</code>, <code>(5, 6)</code>, <code>(6, 5)</code>, <code>(7, 4)</code>, <code>(8, 3)</code>, and <code>(9, 2)</code>. Note that <code>(1, 10)</code> and <code>(10, 1)</code> do not satisfy the conditions because 10 contains 0 in its decimal representation.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Digit DP

We do a digit DP over the decimal representation of $n$ from the least-significant digit to the most-significant digit.

State: `dp[pos][carry][aliveA][aliveB]` = number of ways for the processed suffix.

- `carry` is the carry into the current digit (0 or 1).
- `aliveA/aliveB` indicates whether the number still has digits in higher positions. If `aliveX = 0`, all remaining higher digits must be leading zeros (digit 0), which are not part of the decimal representation.

Transition: choose digits `da` and `db`:

- If `aliveX = 1`, digit is in `[1..9]` (no-zero).
- Otherwise digit is `0`.

They must satisfy `(da + db + carry) % 10 == digit_n[pos]`. After that, `aliveA`/`aliveB` can stay `1` or become `0` (ending the number at this digit).

We append one extra leading digit `0` to $n$ so the last carry is fully handled. The answer is `dp[last][0][0][0]`.

Time complexity is $O(L \cdot 9^2)$ and space complexity is $O(1)$, where $L$ is the number of digits of $n$.

<!-- tabs:start -->

#### Java

```java
class Solution {
	public long countNoZeroPairs(long n) {
		char[] cs = Long.toString(n).toCharArray();
		int m = cs.length;
		int[] digits = new int[m + 1];
		for (int i = 0; i < m; i++) {
			digits[i] = cs[m - 1 - i] - '0';
		}
		digits[m] = 0;

		long[][][] dp = new long[2][2][2];
		dp[0][1][1] = 1;

		for (int pos = 0; pos < m + 1; pos++) {
			long[][][] ndp = new long[2][2][2];
			int target = digits[pos];
			for (int carry = 0; carry <= 1; carry++) {
				for (int aliveA = 0; aliveA <= 1; aliveA++) {
					for (int aliveB = 0; aliveB <= 1; aliveB++) {
						long ways = dp[carry][aliveA][aliveB];
						if (ways == 0) {
							continue;
						}
						int[] aDigits;
						int[] aNext;
						if (aliveA == 1) {
							if (pos == 0) {
								aDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9};
								aNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1};
							} else {
								aDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
								aNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1, 0};
							}
						} else {
							aDigits = new int[] {0};
							aNext = new int[] {0};
						}

						int[] bDigits;
						int[] bNext;
						if (aliveB == 1) {
							if (pos == 0) {
								bDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9};
								bNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1};
							} else {
								bDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
								bNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1, 0};
							}
						} else {
							bDigits = new int[] {0};
							bNext = new int[] {0};
						}

						for (int ai = 0; ai < aDigits.length; ai++) {
							int da = aDigits[ai];
							int na = aNext[ai];
							for (int bi = 0; bi < bDigits.length; bi++) {
								int db = bDigits[bi];
								int nb = bNext[bi];
								int s = da + db + carry;
								if (s % 10 != target) {
									continue;
								}
								int ncarry = s / 10;
								ndp[ncarry][na][nb] += ways;
							}
						}
					}
				}
			}
			dp = ndp;
		}

		return dp[0][0][0];
	}
}

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
