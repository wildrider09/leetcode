---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/10.01.Sorted%20Merge/README_EN.md
---

<!-- problem:start -->

# [10.01. Sorted Merge](https://leetcode.cn/problems/sorted-merge-lcci)

[Chinese Version](/lcci/10.01.Sorted%20Merge/README.md)

## Description

<!-- description:start -->

<p>You are given two sorted arrays, A and B, where A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order.</p>

<p>Initially the number of elements in A and B are&nbsp;<em>m</em>&nbsp;and&nbsp;<em>n</em> respectively.</p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong>

A = [1,2,3,0,0,0], m = 3

B = [2,5,6],       n = 3

<strong>Output:</strong>&nbsp;[1,2,2,3,5,6]</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Two Pointers

We use two pointers $i$ and $j$ to point to the end of arrays $A$ and $B$ respectively, and a pointer $k$ to point to the end of array $A$. Then we traverse arrays $A$ and $B$ from back to front, each time putting the larger element into $A[k]$, then moving pointer $k$ and the pointer of the array with the larger element forward by one position.

The time complexity is $O(m + n)$, and the space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public void merge(int[] A, int m, int[] B, int n) {
        int i = m - 1, j = n - 1;
        for (int k = A.length - 1; k >= 0; --k) {
            if (j < 0 || (i >= 0 && A[i] > B[j])) {
                A[k] = A[i--];
            } else {
                A[k] = B[j--];
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
