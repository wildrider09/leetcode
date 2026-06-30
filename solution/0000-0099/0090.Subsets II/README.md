---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0000-0099/0090.Subsets%20II/README_EN.md
tags:
    - Bit Manipulation
    - Array
    - Backtracking
---

<!-- problem:start -->

# [90. Subsets II](https://leetcode.com/problems/subsets-ii)

[Chinese Version](/solution/0000-0099/0090.Subsets%20II/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> that may contain duplicates, return <em>all possible</em> <span data-keyword="subset"><em>subsets</em></span><em> (the power set)</em>.</p>

<p>The solution set <strong>must not</strong> contain duplicate subsets. Return the solution in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,2]
<strong>Output:</strong> [[],[1],[1,2],[1,2,2],[2],[2,2]]
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [0]
<strong>Output:</strong> [[],[0]]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + DFS

We can first sort the array $\textit{nums}$ to facilitate deduplication.

Then, we design a function $\textit{dfs}(i)$, which represents the current search for subsets starting from the $i$-th element. The execution logic of the function $\textit{dfs}(i)$ is as follows:

If $i \geq n$, it means all elements have been searched, add the current subset to the answer array, and end the recursion.

If $i < n$, add the $i$-th element to the subset, execute $\textit{dfs}(i + 1)$, then remove the $i$-th element from the subset. Next, we check if the $i$-th element is the same as the next element. If they are the same, skip the element in a loop until we find the first element different from the $i$-th element, then execute $\textit{dfs}(i + 1)$.

Finally, we only need to call $\textit{dfs}(0)$ and return the answer array.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $\textit{nums}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int[] nums;

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        this.nums = nums;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= nums.length) {
            ans.add(new ArrayList<>(t));
            return;
        }
        t.add(nums[i]);
        dfs(i + 1);
        int x = t.remove(t.size() - 1);
        while (i + 1 < nums.length && nums[i + 1] == x) {
            ++i;
        }
        dfs(i + 1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Sorting + Binary Enumeration

Similar to Solution 1, we first sort the array $\textit{nums}$ to facilitate deduplication.

Next, we enumerate a binary number $\textit{mask}$ in the range $[0, 2^n)$, where the binary representation of $\textit{mask}$ is an $n$-bit bit string. If the $i$-th bit of $\textit{mask}$ is $1$, it means selecting $\textit{nums}[i]$, and $0$ means not selecting $\textit{nums}[i]$. Note that if the $(i - 1)$-th bit of $\textit{mask}$ is $0$ and $\textit{nums}[i] = \textit{nums}[i - 1]$, it means that the $i$-th element is the same as the $(i - 1)$-th element in the current enumeration scheme. To avoid duplication, we skip this case. Otherwise, we add the subset corresponding to $\textit{mask}$ to the answer array.

After the enumeration, we return the answer array.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $\textit{nums}$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int mask = 0; mask < 1 << n; ++mask) {
            List<Integer> t = new ArrayList<>();
            boolean ok = true;
            for (int i = 0; i < n; ++i) {
                if ((mask >> i & 1) == 1) {
                    if (i > 0 && (mask >> (i - 1) & 1) == 0 && nums[i] == nums[i - 1]) {
                        ok = false;
                        break;
                    }
                    t.add(nums[i]);
                }
            }
            if (ok) {
                ans.add(t);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
