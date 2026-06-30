---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0900-0999/0936.Stamping%20The%20Sequence/README_EN.md
tags:
    - Stack
    - Greedy
    - Queue
    - String
---

<!-- problem:start -->

# [936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence)

[Chinese Version](/solution/0900-0999/0936.Stamping%20The%20Sequence/README.md)

## Description

<!-- description:start -->

<p>You are given two strings <code>stamp</code> and <code>target</code>. Initially, there is a string <code>s</code> of length <code>target.length</code> with all <code>s[i] == &#39;?&#39;</code>.</p>

<p>In one turn, you can place <code>stamp</code> over <code>s</code> and replace every letter in the <code>s</code> with the corresponding letter from <code>stamp</code>.</p>

<ul>
	<li>For example, if <code>stamp = &quot;abc&quot;</code> and <code>target = &quot;abcba&quot;</code>, then <code>s</code> is <code>&quot;?????&quot;</code> initially. In one turn you can:

    <ul>
    	<li>place <code>stamp</code> at index <code>0</code> of <code>s</code> to obtain <code>&quot;abc??&quot;</code>,</li>
    	<li>place <code>stamp</code> at index <code>1</code> of <code>s</code> to obtain <code>&quot;?abc?&quot;</code>, or</li>
    	<li>place <code>stamp</code> at index <code>2</code> of <code>s</code> to obtain <code>&quot;??abc&quot;</code>.</li>
    </ul>
    Note that <code>stamp</code> must be fully contained in the boundaries of <code>s</code> in order to stamp (i.e., you cannot place <code>stamp</code> at index <code>3</code> of <code>s</code>).</li>

</ul>

<p>We want to convert <code>s</code> to <code>target</code> using <strong>at most</strong> <code>10 * target.length</code> turns.</p>

<p>Return <em>an array of the index of the left-most letter being stamped at each turn</em>. If we cannot obtain <code>target</code> from <code>s</code> within <code>10 * target.length</code> turns, return an empty array.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> stamp = &quot;abc&quot;, target = &quot;ababc&quot;
<strong>Output:</strong> [0,2]
<strong>Explanation:</strong> Initially s = &quot;?????&quot;.
- Place stamp at index 0 to get &quot;abc??&quot;.
- Place stamp at index 2 to get &quot;ababc&quot;.
[1,0,2] would also be accepted as an answer, as well as some other answers.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> stamp = &quot;abca&quot;, target = &quot;aabcaca&quot;
<strong>Output:</strong> [3,0,1]
<strong>Explanation:</strong> Initially s = &quot;???????&quot;.
- Place stamp at index 3 to get &quot;???abca&quot;.
- Place stamp at index 0 to get &quot;abcabca&quot;.
- Place stamp at index 1 to get &quot;aabcaca&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= stamp.length &lt;= target.length &lt;= 1000</code></li>
	<li><code>stamp</code> and <code>target</code> consist of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Reverse Thinking + Topological Sorting

If we operate on the sequence in a forward manner, it would be quite complicated because subsequent operations would overwrite previous ones. Therefore, we consider operating on the sequence in a reverse manner, i.e., starting from the target string $target$ and considering the process of turning $target$ into $?????$.

Let's denote the length of the stamp as $m$ and the length of the target string as $n$. If we operate on the target string with the stamp, there are $n-m+1$ starting positions where the stamp can be placed. We can enumerate these $n-m+1$ starting positions and use a method similar to topological sorting to operate in reverse.

Firstly, we clarify that each starting position corresponds to a window of length $m$.

Next, we define the following data structures or variables:

- In-degree array $indeg$, where $indeg[i]$ represents how many characters in the $i$-th window are different from the characters in the stamp. Initially, $indeg[i]=m$. If $indeg[i]=0$, it means that all characters in the $i$-th window are the same as those in the stamp, and we can place the stamp in the $i$-th window.
- Adjacency list $g$, where $g[i]$ represents the set of all windows with different characters from the stamp on the $i$-th position of the target string $target$.
- Queue $q$, used to store the numbers of all windows with an in-degree of $0$.
- Array $vis$, used to mark whether each position of the target string $target$ has been covered.
- Array $ans$, used to store the answer.

Then, we perform topological sorting. In each step of the topological sorting, we take out the window number $i$ at the head of the queue, and put $i$ into the answer array $ans$. Then, we enumerate each position $j$ in the stamp. If the $j$-th position in the $i$-th window has not been covered, we cover it and reduce the in-degree of all windows in the $indeg$ array that are the same as the $j$-th position in the $i$-th window by $1$. If the in-degree of a window becomes $0$, we put it into the queue $q$ for processing next time.

After the topological sorting is over, if every position of the target string $target$ has been covered, then the answer array $ans$ stores the answer we are looking for. Otherwise, the target string $target$ cannot be covered, and we return an empty array.

The time complexity is $O(n \times (n - m + 1))$, and the space complexity is $O(n \times (n - m + 1))$. Here, $n$ and $m$ are the lengths of the target string $target$ and the stamp, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] movesToStamp(String stamp, String target) {
        int m = stamp.length(), n = target.length();
        int[] indeg = new int[n - m + 1];
        Arrays.fill(indeg, m);
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n - m + 1; ++i) {
            for (int j = 0; j < m; ++j) {
                if (target.charAt(i + j) == stamp.charAt(j)) {
                    if (--indeg[i] == 0) {
                        q.offer(i);
                    }
                } else {
                    g[i + j].add(i);
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        boolean[] vis = new boolean[n];
        while (!q.isEmpty()) {
            int i = q.poll();
            ans.add(i);
            for (int j = 0; j < m; ++j) {
                if (!vis[i + j]) {
                    vis[i + j] = true;
                    for (int k : g[i + j]) {
                        if (--indeg[k] == 0) {
                            q.offer(k);
                        }
                    }
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                return new int[0];
            }
        }
        Collections.reverse(ans);
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
