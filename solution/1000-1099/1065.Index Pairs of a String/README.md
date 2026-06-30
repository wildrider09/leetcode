---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1000-1099/1065.Index%20Pairs%20of%20a%20String/README_EN.md
rating: 1389
source: Biweekly Contest 1 Q2
tags:
    - Trie
    - Array
    - String
    - Sorting
---

<!-- problem:start -->

# [1065. Index Pairs of a String 🔒](https://leetcode.com/problems/index-pairs-of-a-string)

[Chinese Version](/solution/1000-1099/1065.Index%20Pairs%20of%20a%20String/README.md)

## Description

<!-- description:start -->

<p>Given a string <code>text</code> and an array of strings <code>words</code>, return <em>an array of all index pairs </em><code>[i, j]</code><em> so that the substring </em><code>text[i...j]</code><em> is in <code>words</code></em>.</p>

<p>Return the pairs <code>[i, j]</code> in sorted order (i.e., sort them by their first coordinate, and in case of ties sort them by their second coordinate).</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;thestoryofleetcodeandme&quot;, words = [&quot;story&quot;,&quot;fleet&quot;,&quot;leetcode&quot;]
<strong>Output:</strong> [[3,7],[9,13],[10,17]]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;ababa&quot;, words = [&quot;aba&quot;,&quot;ab&quot;]
<strong>Output:</strong> [[0,1],[0,2],[2,3],[2,4]]
<strong>Explanation:</strong> Notice that matches can overlap, see &quot;aba&quot; is found in [0,2] and [2,4].
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 100</code></li>
	<li><code>1 &lt;= words.length &lt;= 20</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 50</code></li>
	<li><code>text</code> and <code>words[i]</code> consist of lowercase English letters.</li>
	<li>All the strings of <code>words</code> are <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Trie {
    Trie[] children = new Trie[26];
    boolean isEnd = false;

    void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
        }
        node.isEnd = true;
    }
}

class Solution {
    public int[][] indexPairs(String text, String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        int n = text.length();
        List<int[]> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            Trie node = trie;
            for (int j = i; j < n; ++j) {
                int idx = text.charAt(j) - 'a';
                if (node.children[idx] == null) {
                    break;
                }
                node = node.children[idx];
                if (node.isEnd) {
                    ans.add(new int[] {i, j});
                }
            }
        }
        return ans.toArray(new int[ans.size()][2]);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- solution:end -->

<!-- problem:end -->
