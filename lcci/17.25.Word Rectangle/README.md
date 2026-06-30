---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.25.Word%20Rectangle/README_EN.md
---

<!-- problem:start -->

# [17.25. Word Rectangle](https://leetcode.cn/problems/word-rectangle-lcci)

[Chinese Version](/lcci/17.25.Word%20Rectangle/README.md)

## Description

<!-- description:start -->

<p>Given a list of millions of words, design an algorithm to create the largest possible rectangle of letters such that every row forms a word (reading left to right) and every column forms a word (reading top to bottom). The words need not be chosen consecutively from the list but all rows must be the same length and all columns must be the same height.</p>
<p>If there are more than one answer, return any one of them. A word can be used more than once.</p>
<p><strong>Example 1:</strong></p>
<pre>

<strong>Input:</strong> [&quot;this&quot;, &quot;real&quot;, &quot;hard&quot;, &quot;trh&quot;, &quot;hea&quot;, &quot;iar&quot;, &quot;sld&quot;]

<strong>Output:

</strong>[

&nbsp; &quot;this&quot;,

&nbsp; &quot;real&quot;,

&nbsp; &quot;hard&quot;

]</pre>

<p><strong>Example 2:</strong></p>
<pre>

<strong>Input:</strong> [&quot;aa&quot;]

<strong>Output: </strong>[&quot;aa&quot;,&quot;aa&quot;]</pre>

<p><strong>Notes: </strong></p>
<ul>
	<li><code>words.length &lt;= 1000</code></li>
	<li><code>words[i].length &lt;= 100</code></li>
	<li>It&#39;s guaranteed that&nbsp;all the words are randomly generated.</li>
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
    boolean isEnd;

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
    private int maxL;
    private int maxS;
    private String[] ans;
    private Trie trie = new Trie();
    private List<String> t = new ArrayList<>();

    public String[] maxRectangle(String[] words) {
        Map<Integer, List<String>> d = new HashMap<>(100);
        for (String w : words) {
            maxL = Math.max(maxL, w.length());
            trie.insert(w);
            d.computeIfAbsent(w.length(), k -> new ArrayList<>()).add(w);
        }
        for (List<String> ws : d.values()) {
            t.clear();
            dfs(ws);
        }
        return ans;
    }

    private void dfs(List<String> ws) {
        if (ws.get(0).length() * maxL <= maxS || t.size() >= maxL) {
            return;
        }
        for (String w : ws) {
            t.add(w);
            int st = check(t);
            if (st == 0) {
                t.remove(t.size() - 1);
                continue;
            }
            if (st == 1 && maxS < t.size() * t.get(0).length()) {
                maxS = t.size() * t.get(0).length();
                ans = t.toArray(new String[0]);
            }
            dfs(ws);
            t.remove(t.size() - 1);
        }
    }

    private int check(List<String> mat) {
        int m = mat.size(), n = mat.get(0).length();
        int ans = 1;
        for (int j = 0; j < n; ++j) {
            Trie node = trie;
            for (int i = 0; i < m; ++i) {
                int idx = mat.get(i).charAt(j) - 'a';
                if (node.children[idx] == null) {
                    return 0;
                }
                node = node.children[idx];
            }
            if (!node.isEnd) {
                ans = 2;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
