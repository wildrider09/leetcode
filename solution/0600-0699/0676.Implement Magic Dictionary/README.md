---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0676.Implement%20Magic%20Dictionary/README_EN.md
tags:
    - Depth-First Search
    - Design
    - Trie
    - Hash Table
    - String
---

<!-- problem:start -->

# [676. Implement Magic Dictionary](https://leetcode.com/problems/implement-magic-dictionary)

[Chinese Version](/solution/0600-0699/0676.Implement%20Magic%20Dictionary/README.md)

## Description

<!-- description:start -->

<p>Design a data structure that is initialized with a list of <strong>different</strong> words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.</p>

<p>Implement the&nbsp;<code>MagicDictionary</code>&nbsp;class:</p>

<ul>
	<li><code>MagicDictionary()</code>&nbsp;Initializes the object.</li>
	<li><code>void buildDict(String[]&nbsp;dictionary)</code>&nbsp;Sets the data structure&nbsp;with an array of distinct strings <code>dictionary</code>.</li>
	<li><code>bool search(String searchWord)</code> Returns <code>true</code> if you can change <strong>exactly one character</strong> in <code>searchWord</code> to match any string in the data structure, otherwise returns <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MagicDictionary&quot;, &quot;buildDict&quot;, &quot;search&quot;, &quot;search&quot;, &quot;search&quot;, &quot;search&quot;]
[[], [[&quot;hello&quot;, &quot;leetcode&quot;]], [&quot;hello&quot;], [&quot;hhllo&quot;], [&quot;hell&quot;], [&quot;leetcoded&quot;]]
<strong>Output</strong>
[null, null, false, true, false, false]

<strong>Explanation</strong>
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict([&quot;hello&quot;, &quot;leetcode&quot;]);
magicDictionary.search(&quot;hello&quot;); // return False
magicDictionary.search(&quot;hhllo&quot;); // We can change the second &#39;h&#39; to &#39;e&#39; to match &quot;hello&quot; so we return True
magicDictionary.search(&quot;hell&quot;); // return False
magicDictionary.search(&quot;leetcoded&quot;); // return False
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;dictionary.length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;dictionary[i].length &lt;= 100</code></li>
	<li><code>dictionary[i]</code> consists of only lower-case English letters.</li>
	<li>All the strings in&nbsp;<code>dictionary</code>&nbsp;are <strong>distinct</strong>.</li>
	<li><code>1 &lt;=&nbsp;searchWord.length &lt;= 100</code></li>
	<li><code>searchWord</code>&nbsp;consists of only lower-case English letters.</li>
	<li><code>buildDict</code>&nbsp;will be called only once before <code>search</code>.</li>
	<li>At most <code>100</code> calls will be made to <code>search</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Trie + DFS

We can use a trie to store all the words in the dictionary. For each word we search, we use depth-first search. Specifically, we start from the root of the trie. For the current letter we are traversing, we first check whether there is a child node that is the same as it. If there is, we continue to traverse downwards. Otherwise, we need to check whether there are remaining modification times. If not, it means that it cannot be matched, so we return false. If there are remaining modification times, we can try to modify the current letter and continue to traverse downwards. If the child node corresponding to the modified letter exists, it means that it can be matched, otherwise it means that it cannot be matched, so we return false. If we traverse to the end of the word and the number of modifications is exactly 1, it means that it can be matched, so we return true.

The time complexity is $O(n \times l + q \times l \times |\Sigma|)$, and the space complexity is $O(n \times l)$, where $n$ and $l$ are the number of words in the dictionary and the average length of the words, respectively, and $q$ is the number of words searched. In addition, $|\Sigma|$ represents the size of the character set. Here, the character set is lowercase English letters, so $|\Sigma|=26$.

<!-- tabs:start -->

#### Java

```java
class Trie {
    private Trie[] children = new Trie[26];
    private boolean isEnd;

    public void insert(String w) {
        Trie node = this;
        for (char c : w.toCharArray()) {
            int i = c - 'a';
            if (node.children[i] == null) {
                node.children[i] = new Trie();
            }
            node = node.children[i];
        }
        node.isEnd = true;
    }

    public boolean search(String w) {
        return dfs(w, 0, this, 0);
    }

    private boolean dfs(String w, int i, Trie node, int diff) {
        if (i == w.length()) {
            return diff == 1 && node.isEnd;
        }
        int j = w.charAt(i) - 'a';
        if (node.children[j] != null) {
            if (dfs(w, i + 1, node.children[j], diff)) {
                return true;
            }
        }
        if (diff == 0) {
            for (int k = 0; k < 26; k++) {
                if (k != j && node.children[k] != null) {
                    if (dfs(w, i + 1, node.children[k], 1)) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
}

class MagicDictionary {
    private Trie trie = new Trie();

    public MagicDictionary() {
    }

    public void buildDict(String[] dictionary) {
        for (String w : dictionary) {
            trie.insert(w);
        }
    }

    public boolean search(String searchWord) {
        return trie.search(searchWord);
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
