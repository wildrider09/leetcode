---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.06.Smallest%20Difference/README_EN.md
---

<!-- problem:start -->

# [16.06. Smallest Difference](https://leetcode.cn/problems/smallest-difference-lcci)

[Chinese Version](/lcci/16.06.Smallest%20Difference/README.md)

## Description

<!-- description:start -->

<p>Given two arrays of integers, compute the pair of values (one value in each array) with the smallest (non-negative) difference. Return the difference.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>{1, 3, 15, 11, 2}, {23, 127, 235, 19, 8}

<strong>Output: </strong> 3, the pair (11, 8)

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 100000</code></li>
	<li><code>-2147483648 &lt;= a[i], b[i] &lt;= 2147483647</code></li>
	<li>The result is in the range [-2147483648, 2147483647]</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + Binary Search

We can sort the array $b$, and for each element $x$ in array $a$, perform a binary search in array $b$ to find the element $y$ closest to $x$. Then, the absolute difference between $x$ and $y$ is the absolute difference between $x$ and the closest element in $b$.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of array $b$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int smallestDifference(int[] a, int[] b) {
        Arrays.sort(b);
        long ans = Long.MAX_VALUE;
        for (int x : a) {
            int j = search(b, x);
            if (j < b.length) {
                ans = Math.min(ans, (long) b[j] - x);
            }
            if (j > 0) {
                ans = Math.min(ans, (long) x - b[j - 1]);
            }
        }
        return (int) ans;
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Sorting + Two Pointers

We can sort both arrays $a$ and $b$, and use two pointers $i$ and $j$ to maintain the current positions in the two arrays. Initially, $i$ and $j$ point to the beginning of arrays $a$ and $b$, respectively. At each step, we calculate the absolute difference between $a[i]$ and $b[j]$, and update the answer. If one of the elements pointed to by $i$ and $j$ is smaller than the other, we move the pointer pointing to the smaller element forward by one step. The traversal ends when at least one of the pointers goes beyond the array range.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of arrays $a$ and $b$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int smallestDifference(int[] a, int[] b) {
        Arrays.sort(a);
        Arrays.sort(b);
        int i = 0, j = 0;
        long ans = Long.MAX_VALUE;
        while (i < a.length && j < b.length) {
            ans = Math.min(ans, Math.abs((long) a[i] - (long) b[j]));
            if (a[i] < b[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return (int) ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
