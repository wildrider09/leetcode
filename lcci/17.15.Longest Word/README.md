---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.15.Longest%20Word/README_EN.md
---

<!-- problem:start -->

# [17.15. Longest Word](https://leetcode.cn/problems/longest-word-lcci)

[Chinese Version](/lcci/17.15.Longest%20Word/README.md)

## Description

<!-- description:start -->

<p>Given a list of words, write a program to find the longest word made of other words in the list. If there are more than one answer, return the one that has smallest lexicographic order. If no answer, return an empty string.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong> [&quot;cat&quot;,&quot;banana&quot;,&quot;dog&quot;,&quot;nana&quot;,&quot;walk&quot;,&quot;walker&quot;,&quot;dogwalker&quot;]

<strong>Output: </strong> &quot;dogwalker&quot;

<strong>Explanation: </strong> &quot;dogwalker&quot; can be made of &quot;dog&quot; and &quot;walker&quot;.

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>0 &lt;= len(words) &lt;= 100</code></li>
	<li><code>1 &lt;= len(words[i]) &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Sorting + DFS

Note that in the problem, each word can actually be reused.

We can use a hash table $\textit{s}$ to store all the words, then sort the words in descending order of length, and if the lengths are the same, sort them in ascending lexicographical order.

Next, we iterate through the sorted list of words. For each word $\textit{w}$, we first remove it from the hash table $\textit{s}$, then use depth-first search $\textit{dfs}$ to determine if $\textit{w}$ can be composed of other words. If it can, we return $\textit{w}$.

The execution logic of the function $\textit{dfs}$ is as follows:

- If $\textit{w}$ is empty, return $\text{true}$;
- Iterate through all prefixes of $\textit{w}$. If a prefix is in the hash table $\textit{s}$ and $\textit{dfs}$ returns $\text{true}$, then return $\text{true}$;
- If no prefix meets the condition, return $\text{false}$.

If no word meets the condition, return an empty string.

The time complexity is $O(m \times n \times \log n + n \times 2^M)$, and the space complexity is $O(m \times n)$. Here, $n$ and $m$ are the length of the word list and the average length of the words, respectively, and $M$ is the length of the longest word.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Set<String> s = new HashSet<>();

    public String longestWord(String[] words) {
        for (String w : words) {
            s.add(w);
        }
        Arrays.sort(words, (a, b) -> {
            if (a.length() != b.length()) {
                return b.length() - a.length();
            }
            return a.compareTo(b);
        });
        for (String w : words) {
            s.remove(w);
            if (dfs(w)) {
                return w;
            }
        }
        return "";
    }

    private boolean dfs(String w) {
        if (w.length() == 0) {
            return true;
        }
        for (int k = 1; k <= w.length(); ++k) {
            if (s.contains(w.substring(0, k)) && dfs(w.substring(k))) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
