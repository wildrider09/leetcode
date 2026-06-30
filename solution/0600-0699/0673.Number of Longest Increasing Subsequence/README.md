---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0673.Number%20of%20Longest%20Increasing%20Subsequence/README_EN.md
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence)

[Chinese Version](/solution/0600-0699/0673.Number%20of%20Longest%20Increasing%20Subsequence/README.md)

## Description

<!-- description:start -->

<p>Given an integer array&nbsp;<code>nums</code>, return <em>the number of longest increasing subsequences.</em></p>

<p><strong>Notice</strong> that the sequence has to be <strong>strictly</strong> increasing.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,5,4,7]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,2,2,2,2]
<strong>Output:</strong> 5
<strong>Explanation:</strong> The length of the longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li>The answer is guaranteed to fit inside a 32-bit integer.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i]$ as the length of the longest increasing subsequence ending with $nums[i]$, and $cnt[i]$ as the number of longest increasing subsequences ending with $nums[i]$. Initially, $f[i]=1$, $cnt[i]=1$. Also, we define $mx$ as the length of the longest increasing subsequence, and $ans$ as the number of longest increasing subsequences.

For each $nums[i]$, we enumerate all elements $nums[j]$ in $nums[0:i-1]$. If $nums[j] < nums[i]$, then $nums[i]$ can be appended after $nums[j]$ to form a longer increasing subsequence. If $f[i] < f[j] + 1$, it means the length of the longest increasing subsequence ending with $nums[i]$ has increased, so we need to update $f[i]=f[j]+1$ and $cnt[i]=cnt[j]$. If $f[i]=f[j]+1$, it means we have found a longest increasing subsequence with the same length as before, so we need to increase $cnt[i]$ by $cnt[j]$. Then, if $mx < f[i]$, it means the length of the longest increasing subsequence has increased, so we need to update $mx=f[i]$ and $ans=cnt[i]$. If $mx=f[i]$, it means we have found a longest increasing subsequence with the same length as before, so we need to increase $ans$ by $cnt[i]$.

Finally, we return $ans$.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length;
        int[] f = new int[n];
        int[] cnt = new int[n];
        int mx = 0, ans = 0;
        for (int i = 0; i < n; ++i) {
            f[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    if (f[i] < f[j] + 1) {
                        f[i] = f[j] + 1;
                        cnt[i] = cnt[j];
                    } else if (f[i] == f[j] + 1) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (mx < f[i]) {
                mx = f[i];
                ans = cnt[i];
            } else if (mx == f[i]) {
                ans += cnt[i];
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Binary Indexed Tree

We can use a binary indexed tree to maintain the length and count of the longest increasing subsequence in the prefix interval. We remove duplicates from the array $nums$ and sort it to get the array $arr$. Then we enumerate each element $x$ in $nums$, find the position $i$ of $x$ in the array $arr$ by binary search, then query the length and count of the longest increasing subsequence in $[1,i-1]$, denoted as $v$ and $cnt$, then update the length and count of the longest increasing subsequence in $[i]$ to $v+1$ and $\max(cnt,1)$. Finally, we query the length and count of the longest increasing subsequence in $[1,m]$, where $m$ is the length of the array $arr$, which is the answer.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;
    private int[] d;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
        d = new int[n + 1];
    }

    public void update(int x, int v, int cnt) {
        while (x <= n) {
            if (c[x] < v) {
                c[x] = v;
                d[x] = cnt;
            } else if (c[x] == v) {
                d[x] += cnt;
            }
            x += x & -x;
        }
    }

    public int[] query(int x) {
        int v = 0, cnt = 0;
        while (x > 0) {
            if (c[x] > v) {
                v = c[x];
                cnt = d[x];
            } else if (c[x] == v) {
                cnt += d[x];
            }
            x -= x & -x;
        }
        return new int[] {v, cnt};
    }
}

public class Solution {
    public int findNumberOfLIS(int[] nums) {
        // int[] arr = Arrays.stream(nums).distinct().sorted().toArray();
        int[] arr = nums.clone();
        Arrays.sort(arr);
        int m = arr.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        for (int x : nums) {
            int i = Arrays.binarySearch(arr, x) + 1;
            int[] t = tree.query(i - 1);
            int v = t[0];
            int cnt = t[1];
            tree.update(i, v + 1, Math.max(cnt, 1));
        }
        return tree.query(m)[1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
