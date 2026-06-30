---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.14.Smallest%20K/README_EN.md
---

<!-- problem:start -->

# [17.14. Smallest K](https://leetcode.cn/problems/smallest-k-lcci)

[Chinese Version](/lcci/17.14.Smallest%20K/README.md)

## Description

<!-- description:start -->

<p>Design an algorithm to find the smallest K numbers in an array.</p>
<p><strong>Example: </strong></p>
<pre>

<strong>Input: </strong> arr = [1,3,5,7,2,4,6,8], k = 4

<strong>Output: </strong> [1,2,3,4]

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>0 &lt;= len(arr) &lt;= 100000</code></li>
	<li><code>0 &lt;= k &lt;= min(100000, len(arr))</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] smallestK(int[] arr, int k) {
        Arrays.sort(arr);
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            ans[i] = arr[i];
        }
        return ans;
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
    public int[] smallestK(int[] arr, int k) {
        PriorityQueue<Integer> q = new PriorityQueue<>((a, b) -> b - a);
        for (int v : arr) {
            q.offer(v);
            if (q.size() > k) {
                q.poll();
            }
        }
        int[] ans = new int[k];
        int i = 0;
        while (!q.isEmpty()) {
            ans[i++] = q.poll();
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
