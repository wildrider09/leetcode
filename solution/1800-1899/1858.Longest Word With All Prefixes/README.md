---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1800-1899/1858.Longest%20Word%20With%20All%20Prefixes/README_EN.md
tags:
    - Depth-First Search
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [1858. Longest Word With All Prefixes 🔒](https://leetcode.com/problems/longest-word-with-all-prefixes)

[Chinese Version](/solution/1800-1899/1858.Longest%20Word%20With%20All%20Prefixes/README.md)

## Description

<!-- description:start -->

<p>Given an array of strings <code>words</code>, find the <strong>longest</strong> string in <code>words</code> such that <strong>every prefix</strong> of it is also in <code>words</code>.</p>

<ul>
	<li>For example, let <code>words = [&quot;a&quot;, &quot;app&quot;, &quot;ap&quot;]</code>. The string <code>&quot;app&quot;</code> has prefixes <code>&quot;ap&quot;</code> and <code>&quot;a&quot;</code>, all of which are in <code>words</code>.</li>
</ul>

<p>Return <em>the string described above. If there is more than one string with the same length, return the <strong>lexicographically smallest</strong> one, and if no string exists, return </em><code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;k&quot;,&quot;ki&quot;,&quot;kir&quot;,&quot;kira&quot;, &quot;kiran&quot;]
<strong>Output:</strong> &quot;kiran&quot;
<strong>Explanation:</strong> &quot;kiran&quot; has prefixes &quot;kira&quot;, &quot;kir&quot;, &quot;ki&quot;, and &quot;k&quot;, and all of them appear in words.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;a&quot;, &quot;banana&quot;, &quot;app&quot;, &quot;appl&quot;, &quot;ap&quot;, &quot;apply&quot;, &quot;apple&quot;]
<strong>Output:</strong> &quot;apple&quot;
<strong>Explanation:</strong> Both &quot;apple&quot; and &quot;apply&quot; have all their prefixes in words.
However, &quot;apple&quot; is lexicographically smaller, so we return that.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;abc&quot;, &quot;bc&quot;, &quot;ab&quot;, &quot;qwe&quot;]
<strong>Output:</strong> &quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sum(words[i].length) &lt;= 10<sup>5</sup></code></li>
	<li><code>words[i]</code> consists only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Trie

We define a Trie where each node has two attributes: a child node array $\textit{children}$ of length $26$, and a flag $\textit{isEnd}$ indicating whether the node marks the end of a word.

We iterate over $\textit{words}$, and for each word $w$, we traverse from the root node. If the child node array of the current node does not contain the first character of $w$, we create a new node, then continue traversing the next character of $w$. After traversing all characters of $w$, we set the $\textit{isEnd}$ flag of the current node to $\texttt{true}$.

Next, we iterate over $\textit{words}$ again, and for each word $w$, we traverse from the root node. If the $\textit{isEnd}$ field of a node in the child node array is $\texttt{false}$, it means some prefix of $w$ is not in $\textit{words}$, and we return $\texttt{false}$. Otherwise, we continue traversing the next character of $w$, and after traversing all characters, we return $\texttt{true}$.

The time complexity is $O(\sum_{w \in \textit{words}} |w|)$, and the space complexity is $O(\sum_{w \in \textit{words}} |w|)$, where $|w|$ is the length of word $w$.

<!-- tabs:start -->

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private boolean isEnd;

    public Trie() {
    }

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new Trie();
            }
            node = node.children[idx];
        }
        node.isEnd = true;
    }

    public boolean search(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int idx = c - 'a';
            node = node.children[idx];
            if (!node.isEnd) {
                return false;
            }
        }
        return true;
    }
}

class Solution {
    public String longestWord(String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        String ans = "";
        for (String w : words) {
            if ((w.length() > ans.length() || (w.length() == ans.length() && w.compareTo(ans) < 0))
                && trie.search(w)) {
                ans = w;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
