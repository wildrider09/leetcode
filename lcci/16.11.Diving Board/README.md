---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.11.Diving%20Board/README_EN.md
---

<!-- problem:start -->

# [16.11. Diving Board](https://leetcode.cn/problems/diving-board-lcci)

[Chinese Version](/lcci/16.11.Diving%20Board/README.md)

## Description

<!-- description:start -->

<p>You are building a diving board by placing a bunch of planks of wood end-to-end. There are two types of planks, one of length <code>shorter</code> and one of length <code>longer</code>. You must use exactly <code>K</code> planks of wood. Write a method to generate all possible lengths for the diving board.</p>

<p>return all lengths in non-decreasing order.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>

shorter = 1

longer = 2

k = 3

<strong>Output: </strong> {3,4,5,6}

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li>0 &lt; shorter &lt;= longer</li>
	<li>0 &lt;= k &lt;= 100000</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Case Analysis

If $k=0$, there is no solution, and we can directly return an empty list.

If $shorter=longer$, we can only use a board with length $longer \times k$, so we directly return a list with length $longer \times k$.

Otherwise, we can use a board with length $shorter \times (k-i) + longer \times i$, where $0 \leq i \leq k$. We enumerate $i$ in the range $[0, k]$, and calculate the corresponding length. For different values of $i$, we will not get the same length, because if $0 \leq i \lt j \leq k$, then the difference in length is $(i - j) \times (longer - shorter) \lt 0$. Therefore, for different values of $i$, we will get different lengths.

The time complexity is $O(k)$, where $k$ is the number of boards. Ignoring the space consumption of the answer, the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] divingBoard(int shorter, int longer, int k) {
        if (k == 0) {
            return new int[0];
        }
        if (longer == shorter) {
            return new int[] {longer * k};
        }
        int[] ans = new int[k + 1];
        for (int i = 0; i < k + 1; ++i) {
            ans[i] = longer * i + shorter * (k - i);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
