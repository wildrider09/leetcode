---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.10.Find%20Majority%20Element/README_EN.md
---

<!-- problem:start -->

# [17.10. Find Majority Element](https://leetcode.cn/problems/find-majority-element-lcci)

[Chinese Version](/lcci/17.10.Find%20Majority%20Element/README.md)

## Description

<!-- description:start -->

<p>A majority element is an element that makes up more than half of the items in an array. Given a positive integers array, find the majority element. If there is no majority element, return -1. Do this in O(N) time and O(1) space.</p>

<p><strong>Example 1: </strong></p>

<pre>

<strong>Input: </strong>[1,2,5,9,5,9,5,5,5]

<strong>Output: </strong>5</pre>

<p>&nbsp;</p>

<p><strong>Example 2: </strong></p>

<pre>

<strong>Input: </strong>[3,2]

<strong>Output: </strong>-1</pre>

<p>&nbsp;</p>

<p><strong>Example 3: </strong></p>

<pre>

<strong>Input: </strong>[2,2,1,1,1,2,2]

<strong>Output: </strong>2

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cnt = 0, m = 0;
        for (int v : nums) {
            if (cnt == 0) {
                m = v;
                cnt = 1;
            } else {
                cnt += (m == v ? 1 : -1);
            }
        }
        cnt = 0;
        for (int v : nums) {
            if (m == v) {
                ++cnt;
            }
        }
        return cnt > nums.length / 2 ? m : -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
