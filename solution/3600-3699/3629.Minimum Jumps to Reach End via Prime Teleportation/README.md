---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3600-3699/3629.Minimum%20Jumps%20to%20Reach%20End%20via%20Prime%20Teleportation/README_EN.md
rating: 2139
source: Weekly Contest 460 Q3
tags:
    - Breadth-First Search
    - Array
    - Hash Table
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3629. Minimum Jumps to Reach End via Prime Teleportation](https://leetcode.com/problems/minimum-jumps-to-reach-end-via-prime-teleportation)

[Chinese Version](/solution/3600-3699/3629.Minimum%20Jumps%20to%20Reach%20End%20via%20Prime%20Teleportation/README.md)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code> of length <code>n</code>.</p>

<p>You start at index 0, and your goal is to reach index <code>n - 1</code>.</p>

<p>From any index <code>i</code>, you may perform one of the following operations:</p>

<ul>
	<li><strong>Adjacent Step</strong>: Jump to index <code>i + 1</code> or <code>i - 1</code>, if the index is within bounds.</li>
	<li><strong>Prime Teleportation</strong>: If <code>nums[i]</code> is a <span data-keyword="prime-number">prime number</span> <code>p</code>, you may instantly jump to any index <code>j != i</code> such that <code>nums[j] % p == 0</code>.</li>
</ul>

<p>Return the <strong>minimum</strong> number of jumps required to reach index <code>n - 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [1,2,4,6]</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>One optimal sequence of jumps is:</p>

<ul>
	<li>Start at index <code>i = 0</code>. Take an adjacent step to index 1.</li>
	<li>At index <code>i = 1</code>, <code>nums[1] = 2</code> is a prime number. Therefore, we teleport to index <code>i = 3</code> as <code>nums[3] = 6</code> is divisible by 2.</li>
</ul>

<p>Thus, the answer is 2.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [2,3,4,7,9]</span></p>

<p><strong>Output:</strong> <span class="example-io">2</span></p>

<p><strong>Explanation:</strong></p>

<p>One optimal sequence of jumps is:</p>

<ul>
	<li>Start at index <code>i = 0</code>. Take an adjacent step to index <code>i = 1</code>.</li>
	<li>At index <code>i = 1</code>, <code>nums[1] = 3</code> is a prime number. Therefore, we teleport to index <code>i = 4</code> since <code>nums[4] = 9</code> is divisible by 3.</li>
</ul>

<p>Thus, the answer is 2.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [4,6,5,8]</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>Since no teleportation is possible, we move through <code>0 &rarr; 1 &rarr; 2 &rarr; 3</code>. Thus, the answer is 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Preprocessing + BFS

First, we preprocess the list of prime factors for every number up to $10^6$ and store them in $\textit{factors}$.

Then we build a graph $g$. For each index $i$ and each $p \in \textit{factors}[nums[i]]$, we add $i$ to $g[p]$. In this way, we obtain the list of indices that can be reached by teleportation through each prime number $p$.

Next, we use breadth-first search to find the minimum number of jumps. We maintain a queue $q$ to store the indices that can currently be reached, with only index $0$ in $q$ initially. Each time we pop an index $i$ from $q$, if $i$ is the target index $n - 1$, we return the current number of jumps. Otherwise, we add all indices in $g[nums[i]]$ to $q$ and remove them from $g[nums[i]]$ to avoid repeated visits. At the same time, we also add the adjacent indices $i + 1$ and $i - 1$ to $q$ if they are within bounds.

The time complexity is $O(n \log M)$, and the space complexity is $O(n \log M)$, where $n$ is the length of the array, and $M$ is the maximum value in the array.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private static final int mx = 1000001;
    private static final List<Integer>[] factors = new List[mx];

    static {
        for (int i = 0; i < mx; i++) {
            factors[i] = new ArrayList<>();
        }
        for (int i = 2; i < mx; i++) {
            if (factors[i].isEmpty()) {
                for (int j = i; j < mx; j += i) {
                    factors[j].add(i);
                }
            }
        }
    }

    public int minJumps(int[] nums) {
        int n = nums.length;
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            for (int p : factors[x]) {
                g.computeIfAbsent(p, k -> new ArrayList<>()).add(i);
            }
        }
        int ans = 0;
        boolean[] vis = new boolean[n];
        vis[0] = true;
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        while (true) {
            Deque<Integer> nq = new ArrayDeque<>();
            for (int i : q) {
                if (i == n - 1) {
                    return ans;
                }
                List<Integer> idx = g.getOrDefault(nums[i], new ArrayList<>());
                idx.add(i + 1);
                if (i > 0) {
                    idx.add(i - 1);
                }
                for (int j : idx) {
                    if (!vis[j]) {
                        vis[j] = true;
                        nq.offer(j);
                    }
                }
                idx.clear();
            }
            q = nq;
            ans++;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
