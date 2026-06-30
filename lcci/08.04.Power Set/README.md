---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/08.04.Power%20Set/README_EN.md
---

<!-- problem:start -->

# [08.04. Power Set](https://leetcode.cn/problems/power-set-lcci)

[Chinese Version](/lcci/08.04.Power%20Set/README.md)

## Description

<!-- description:start -->

<p>Write a method to return all subsets of a set. The elements in a set are&nbsp;pairwise distinct.</p>

<p>Note: The result set should not contain duplicated subsets.</p>

<p><strong>Example:</strong></p>

<pre>

<strong> Input</strong>:  nums = [1,2,3]

<strong> Output</strong>: 

[

  [3],

&nbsp; [1],

&nbsp; [2],

&nbsp; [1,2,3],

&nbsp; [1,3],

&nbsp; [2,3],

&nbsp; [1,2],

&nbsp; []

]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursive Enumeration

We design a recursive function $dfs(u, t)$, where $u$ is the index of the current element being enumerated, and $t$ is the current subset.

For the current element with index $u$, we can choose to add it to the subset $t$, or we can choose not to add it to the subset $t$. Recursively making these two choices will yield all subsets.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array. Each element in the array has two states, namely chosen or not chosen, for a total of $2^n$ states. Each state requires $O(n)$ time to construct the subset.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private int[] nums;

    public List<List<Integer>> subsets(int[] nums) {
        this.nums = nums;
        dfs(0, new ArrayList<>());
        return ans;
    }

    private void dfs(int u, List<Integer> t) {
        if (u == nums.length) {
            ans.add(new ArrayList<>(t));
            return;
        }
        dfs(u + 1, t);
        t.add(nums[u]);
        dfs(u + 1, t);
        t.remove(t.size() - 1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Binary Enumeration

We can rewrite the recursive process in Method 1 into an iterative form, that is, using binary enumeration to enumerate all subsets.

We can use $2^n$ binary numbers to represent all subsets of $n$ elements. If the $i$-th bit of a binary number `mask` is $1$, it means that the subset contains the $i$-th element $v$ of the array; if it is $0$, it means that the subset does not contain the $i$-th element $v$ of the array.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array. There are a total of $2^n$ subsets, and each subset requires $O(n)$ time to construct.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int mask = 0; mask < 1 << n; ++mask) {
            List<Integer> t = new ArrayList<>();
            for (int i = 0; i < n; ++i) {
                if (((mask >> i) & 1) == 1) {
                    t.add(nums[i]);
                }
            }
            ans.add(t);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
