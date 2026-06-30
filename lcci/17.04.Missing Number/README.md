---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.04.Missing%20Number/README_EN.md
---

<!-- problem:start -->

# [17.04. Missing Number](https://leetcode.cn/problems/missing-number-lcci)

[Chinese Version](/lcci/17.04.Missing%20Number/README.md)

## Description

<!-- description:start -->

<p>An array&nbsp;contains all the integers from 0 to n, except for one number which is missing.&nbsp; Write code to find the missing integer. Can you do it in O(n) time?</p>

<p><strong>Note: </strong>This problem is slightly different from the original one the book.</p>

<p><strong>Example 1: </strong></p>

<pre>

<strong>Input: </strong>[3,0,1]

<strong>Output: </strong>2</pre>

<p>&nbsp;</p>

<p><strong>Example 2: </strong></p>

<pre>

<strong>Input: </strong>[9,6,4,2,3,5,7,0,1]

<strong>Output: </strong>8

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (i != nums[i]) {
                return i;
            }
        }
        return n;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans += i - nums[i];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        int ans = 0;
        for (int i = 1; i <= nums.length; ++i) {
            ans ^= i ^ nums[i - 1];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
