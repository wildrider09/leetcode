---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/2900-2999/2907.Maximum%20Profitable%20Triplets%20With%20Increasing%20Prices%20I/README_EN.md
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [2907. Maximum Profitable Triplets With Increasing Prices I 🔒](https://leetcode.com/problems/maximum-profitable-triplets-with-increasing-prices-i)

[Chinese Version](/solution/2900-2999/2907.Maximum%20Profitable%20Triplets%20With%20Increasing%20Prices%20I/README.md)

## Description

<!-- description:start -->

<p>Given the <strong>0-indexed</strong> arrays <code>prices</code> and <code>profits</code> of length <code>n</code>. There are <code>n</code> items in an store where the <code>i<sup>th</sup></code> item has a price of <code>prices[i]</code> and a profit of <code>profits[i]</code>.</p>

<p>We have to pick three items with the following condition:</p>

<ul>
	<li><code>prices[i] &lt; prices[j] &lt; prices[k]</code> where <code>i &lt; j &lt; k</code>.</li>
</ul>

<p>If we pick items with indices <code>i</code>, <code>j</code> and <code>k</code> satisfying the above condition, the profit would be <code>profits[i] + profits[j] + profits[k]</code>.</p>

<p>Return<em> the <strong>maximum profit</strong> we can get, and </em><code>-1</code><em> if it&#39;s not possible to pick three items with the given condition.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> prices = [10,2,3,4], profits = [100,2,7,10]
<strong>Output:</strong> 19
<strong>Explanation:</strong> We can&#39;t pick the item with index i=0 since there are no indices j and k such that the condition holds.
So the only triplet we can pick, are the items with indices 1, 2 and 3 and it&#39;s a valid pick since prices[1] &lt; prices[2] &lt; prices[3].
The answer would be sum of their profits which is 2 + 7 + 10 = 19.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> prices = [1,2,3,4,5], profits = [1,5,3,4,6]
<strong>Output:</strong> 15
<strong>Explanation:</strong> We can select any triplet of items since for each triplet of indices i, j and k such that i &lt; j &lt; k, the condition holds.
Therefore the maximum profit we can get would be the 3 most profitable items which are indices 1, 3 and 4.
The answer would be sum of their profits which is 5 + 4 + 6 = 15.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> prices = [4,3,2,1], profits = [33,20,19,87]
<strong>Output:</strong> -1
<strong>Explanation:</strong> We can&#39;t select any triplet of indices such that the condition holds, so we return -1.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= prices.length == profits.length &lt;= 2000</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= profits[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumerate the Middle Element

We can enumerate the middle element $profits[j]$, and then enumerate the left element $profits[i]$ and the right element $profits[k]$. For each $profits[j]$, we need to find the maximum $profits[i]$ and the maximum $profits[k]$ such that $prices[i] < prices[j] < prices[k]$. We define $left$ as the maximum value on the left of $profits[j]$, and $right$ as the maximum value on the right of $profits[j]$. If they exist, we update the answer as $ans = \max(ans, left + profits[j] + right)$.

The time complexity is $O(n^2)$, where $n$ is the length of the array. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int ans = -1;
        for (int j = 0; j < n; ++j) {
            int left = 0, right = 0;
            for (int i = 0; i < j; ++i) {
                if (prices[i] < prices[j]) {
                    left = Math.max(left, profits[i]);
                }
            }
            for (int k = j + 1; k < n; ++k) {
                if (prices[j] < prices[k]) {
                    right = Math.max(right, profits[k]);
                }
            }
            if (left > 0 && right > 0) {
                ans = Math.max(ans, left + profits[j] + right);
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

We can use two Binary Indexed Trees (BITs) to maintain the maximum profit on the left and right of each price, respectively. Then, we enumerate the middle price, query the maximum profit on both sides through the BIT, and finally take the maximum value.

The time complexity is $O(n \times \log M)$, and the space complexity is $O(M)$. Here, $n$ is the length of the array $prices$, and $M$ is the maximum value in the array $prices$. In this problem, $M \le 10^6$.

Since the length of $prices$ does not exceed $2000$, and the value of $prices[i]$ reaches $10^6$, we can discretize $prices$, which can reduce the space complexity to $O(n)$.

<!-- tabs:start -->

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int m = 0;
        for (int x : prices) {
            m = Math.max(m, x);
        }
        BinaryIndexedTree tree1 = new BinaryIndexedTree(m + 1);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = prices[i];
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - prices[i];
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + profits[i] + right[i]);
            }
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
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = 0;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public int maxProfit(int[] prices, int[] profits) {
        int n = prices.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int[] s = prices.clone();
        Arrays.sort(s);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || s[i] != s[i - 1]) {
                s[m++] = s[i];
            }
        }
        BinaryIndexedTree tree1 = new BinaryIndexedTree(m + 1);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(m + 1);
        for (int i = 0; i < n; ++i) {
            int x = search(s, prices[i], m);
            left[i] = tree1.query(x - 1);
            tree1.update(x, profits[i]);
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = m + 1 - search(s, prices[i], m);
            right[i] = tree2.query(x - 1);
            tree2.update(x, profits[i]);
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 0 && right[i] > 0) {
                ans = Math.max(ans, left[i] + profits[i] + right[i]);
            }
        }
        return ans;
    }

    private int search(int[] nums, int x, int r) {
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l + 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
