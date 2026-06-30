---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.21.Volume%20of%20Histogram/README_EN.md
---

<!-- problem:start -->

# [17.21. Volume of Histogram](https://leetcode.cn/problems/volume-of-histogram-lcci)

[Chinese Version](/lcci/17.21.Volume%20of%20Histogram/README.md)

## Description

<!-- description:start -->

<p>Imagine a histogram (bar graph). Design an algorithm to compute the volume of water it could hold if someone poured water across the top. You can assume that each histogram bar has width 1.</p>

![](https://fastly.jsdelivr.net/gh/doocs/leetcode@main/lcci/17.21.Volume%20of%20Histogram/images/rainwatertrap.png)

<p><small>The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of water (blue section) are being trapped. Thanks <strong>Marcos</strong> for contributing this image!</small></p>

<p><strong>Example:</strong></p>

<pre>

<strong>Input:</strong> [0,1,0,2,1,0,1,3,2,1,2,1]

<strong>Output:</strong> 6</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n < 3) {
            return 0;
        }
        int[] left = new int[n];
        int[] right = new int[n];
        left[0] = height[0];
        right[n - 1] = height[n - 1];
        for (int i = 1; i < n; ++i) {
            left[i] = Math.max(left[i - 1], height[i]);
            right[n - i - 1] = Math.max(right[n - i], height[n - i - 1]);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.min(left[i], right[i]) - height[i];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
