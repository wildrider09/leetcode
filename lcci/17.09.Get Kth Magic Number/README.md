---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.09.Get%20Kth%20Magic%20Number/README_EN.md
---

<!-- problem:start -->

# [17.09. Get Kth Magic Number](https://leetcode.cn/problems/get-kth-magic-number-lcci)

## Description

<!-- description:start -->

<p>Design an algorithm to find the kth number such that the only prime factors are 3, 5, and 7. Note that 3, 5, and 7 do not have to be factors, but it should not have any other prime factors. For example, the first several multiples would be (in order) 1, 3, 5, 7, 9, 15, 21.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong>Input: </strong>k = 5

<strong>Output: </strong>9

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final int[] FACTORS = new int[] {3, 5, 7};

    public int getKthMagicNumber(int k) {
        PriorityQueue<Long> q = new PriorityQueue<>();
        Set<Long> vis = new HashSet<>();
        q.offer(1L);
        vis.add(1L);
        while (--k > 0) {
            long cur = q.poll();
            for (int f : FACTORS) {
                long nxt = cur * f;
                if (!vis.contains(nxt)) {
                    q.offer(nxt);
                    vis.add(nxt);
                }
            }
        }
        long ans = q.poll();
        return (int) ans;
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
    public int getKthMagicNumber(int k) {
        int[] dp = new int[k + 1];
        Arrays.fill(dp, 1);
        int p3 = 1, p5 = 1, p7 = 1;
        for (int i = 2; i <= k; ++i) {
            int a = dp[p3] * 3, b = dp[p5] * 5, c = dp[p7] * 7;
            int v = Math.min(Math.min(a, b), c);
            dp[i] = v;
            if (v == a) {
                ++p3;
            }
            if (v == b) {
                ++p5;
            }
            if (v == c) {
                ++p7;
            }
        }
        return dp[k];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
