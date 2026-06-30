---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0500-0599/0581.Shortest%20Unsorted%20Continuous%20Subarray/README_EN.md
tags:
    - Stack
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray)

[Chinese Version](/solution/0500-0599/0581.Shortest%20Unsorted%20Continuous%20Subarray/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code>, you need to find one <b>continuous subarray</b> such that if you only sort this subarray in non-decreasing order, then the whole array will be sorted in non-decreasing order.</p>

<p>Return <em>the shortest such subarray and output its length</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,6,4,8,10,9,15]
<strong>Output:</strong> 5
<strong>Explanation:</strong> You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4]
<strong>Output:</strong> 0
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Can you solve it in <code>O(n)</code> time complexity?

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting

We can first sort the array, and then compare the sorted array with the original array to find the leftmost and rightmost positions where they differ. The length between them is the length of the shortest unsorted continuous subarray.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] arr = nums.clone();
        Arrays.sort(arr);
        int l = 0, r = arr.length - 1;
        while (l <= r && nums[l] == arr[l]) {
            l++;
        }
        while (l <= r && nums[r] == arr[r]) {
            r--;
        }
        return r - l + 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Maintaining the Maximum Value on the Left and the Minimum Value on the Right

We can traverse the array from left to right and maintain a maximum value $mx$. If the current value is less than $mx$, it means that the current value is not in the correct position, and we update the right boundary $r$ to the current position. Similarly, we can traverse the array from right to left and maintain a minimum value $mi$. If the current value is greater than $mi$, it means that the current value is not in the correct position, and we update the left boundary $l$ to the current position. At initialization, we set $l$ and $r$ to $-1$. If $l$ and $r$ are not updated, it means that the array is already sorted, and we return $0$. Otherwise, we return $r - l + 1$.

The time complexity is $O(n)$, and the space complexity is $O(1)$. Here, $n$ is the length of the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        final int inf = 1 << 30;
        int n = nums.length;
        int l = -1, r = -1;
        int mi = inf, mx = -inf;
        for (int i = 0; i < n; ++i) {
            if (mx > nums[i]) {
                r = i;
            } else {
                mx = nums[i];
            }
            if (mi < nums[n - i - 1]) {
                l = n - i - 1;
            } else {
                mi = nums[n - i - 1];
            }
        }
        return r == -1 ? 0 : r - l + 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
