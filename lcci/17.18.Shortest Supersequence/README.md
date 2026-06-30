---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.18.Shortest%20Supersequence/README_EN.md
---

<!-- problem:start -->

# [17.18. Shortest Supersequence](https://leetcode.cn/problems/shortest-supersequence-lcci)

[Chinese Version](/lcci/17.18.Shortest%20Supersequence/README.md)

## Description

<!-- description:start -->

<p>You are given two arrays, one shorter (with all distinct elements) and one longer. Find the shortest subarray in the longer array that contains all the elements in the shorter array. The items can appear in any order.</p>
<p>Return the indexes of the leftmost and the rightmost elements of the array. If there are more than one answer, return the one that has the smallest left index. If there is no answer, return an empty array.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong>Input:</strong>

big = [7,5,9,0,2,1,3,<strong>5,7,9,1</strong>,1,5,8,8,9,7]

small = [1,5,9]

<strong>Output: </strong>[7,10]</pre>

<p><strong>Example 2:</strong></p>
<pre>

<strong>Input:</strong>

big = [1,2,3]

small = [4]

<strong>Output: </strong>[]</pre>

<p><strong>Note: </strong></p>
<ul>
	<li><code>big.length&nbsp;&lt;= 100000</code></li>
	<li><code>1 &lt;= small.length&nbsp;&lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] shortestSeq(int[] big, int[] small) {
        int cnt = small.length;
        Map<Integer, Integer> need = new HashMap<>(cnt);
        Map<Integer, Integer> window = new HashMap<>(cnt);
        for (int x : small) {
            need.put(x, 1);
        }
        int k = -1, mi = 1 << 30;
        for (int i = 0, j = 0; i < big.length; ++i) {
            window.merge(big[i], 1, Integer::sum);
            if (need.getOrDefault(big[i], 0) >= window.get(big[i])) {
                --cnt;
            }
            while (cnt == 0) {
                if (i - j + 1 < mi) {
                    mi = i - j + 1;
                    k = j;
                }
                if (need.getOrDefault(big[j], 0) >= window.get(big[j])) {
                    ++cnt;
                }
                window.merge(big[j++], -1, Integer::sum);
            }
        }
        return k < 0 ? new int[0] : new int[] {k, k + mi - 1};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
