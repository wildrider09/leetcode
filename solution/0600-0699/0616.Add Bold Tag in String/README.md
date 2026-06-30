---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0600-0699/0616.Add%20Bold%20Tag%20in%20String/README_EN.md
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - String Matching
---

<!-- problem:start -->

# [616. Add Bold Tag in String 🔒](https://leetcode.com/problems/add-bold-tag-in-string)

[Chinese Version](/solution/0600-0699/0616.Add%20Bold%20Tag%20in%20String/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> and an array of strings <code>words</code>.</p>

<p>You should add a closed pair of bold tag <code>&lt;b&gt;</code> and <code>&lt;/b&gt;</code> to wrap the substrings in <code>s</code> that exist in <code>words</code>.</p>

<ul>
	<li>If two such substrings overlap, you should wrap them together with only one pair of closed bold-tag.</li>
	<li>If two substrings wrapped by bold tags are consecutive, you should combine them.</li>
</ul>

<p>Return <code>s</code> <em>after adding the bold tags</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcxyz123&quot;, words = [&quot;abc&quot;,&quot;123&quot;]
<strong>Output:</strong> &quot;&lt;b&gt;abc&lt;/b&gt;xyz&lt;b&gt;123&lt;/b&gt;&quot;
<strong>Explanation:</strong> The two strings of words are substrings of s as following: &quot;<u>abc</u>xyz<u>123</u>&quot;.
We add &lt;b&gt; before each substring and &lt;/b&gt; after each substring.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaabbb&quot;, words = [&quot;aa&quot;,&quot;b&quot;]
<strong>Output:</strong> &quot;&lt;b&gt;aaabbb&lt;/b&gt;&quot;
<strong>Explanation:</strong> 
&quot;aa&quot; appears as a substring two times: &quot;<u>aa</u>abbb&quot; and &quot;a<u>aa</u>bbb&quot;.
&quot;b&quot; appears as a substring three times: &quot;aaa<u>b</u>bb&quot;, &quot;aaab<u>b</u>b&quot;, and &quot;aaabb<u>b</u>&quot;.
We add &lt;b&gt; before each substring and &lt;/b&gt; after each substring: &quot;&lt;b&gt;a&lt;b&gt;a&lt;/b&gt;a&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&quot;.
Since the first two &lt;b&gt;&#39;s overlap, we merge them: &quot;&lt;b&gt;aaa&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&lt;b&gt;b&lt;/b&gt;&quot;.
Since now the four &lt;b&gt;&#39;s are consecutive, we merge them: &quot;&lt;b&gt;aaabbb&lt;/b&gt;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>0 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li><code>s</code> and <code>words[i]</code> consist of English letters and digits.</li>
	<li>All the values of <code>words</code> are <strong>unique</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Note:</strong> This question is the same as <a href="https://leetcode.com/problems/bold-words-in-string/description/" target="_blank">758. Bold Words in String</a>.</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Trie {
    Trie[] children = new Trie[128];
    boolean isEnd;

    public void insert(String word) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
        }
        node.isEnd = true;
    }
}

class Solution {
    public String addBoldTag(String s, String[] words) {
        Trie trie = new Trie();
        for (String w : words) {
            trie.insert(w);
        }
        List<int[]> pairs = new ArrayList<>();
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            Trie node = trie;
            for (int j = i; j < n; ++j) {
                int idx = s.charAt(j);
                if (node.children[idx] == null) {
                    break;
                }
                node = node.children[idx];
                if (node.isEnd) {
                    pairs.add(new int[] {i, j});
                }
            }
        }
        if (pairs.isEmpty()) {
            return s;
        }
        List<int[]> t = new ArrayList<>();
        int st = pairs.get(0)[0], ed = pairs.get(0)[1];
        for (int j = 1; j < pairs.size(); ++j) {
            int a = pairs.get(j)[0], b = pairs.get(j)[1];
            if (ed + 1 < a) {
                t.add(new int[] {st, ed});
                st = a;
                ed = b;
            } else {
                ed = Math.max(ed, b);
            }
        }
        t.add(new int[] {st, ed});
        int i = 0, j = 0;
        StringBuilder ans = new StringBuilder();
        while (i < n) {
            if (j == t.size()) {
                ans.append(s.substring(i));
                break;
            }
            st = t.get(j)[0];
            ed = t.get(j)[1];
            if (i < st) {
                ans.append(s.substring(i, st));
            }
            ++j;
            ans.append("<b>");
            ans.append(s.substring(st, ed + 1));
            ans.append("</b>");
            i = ed + 1;
        }
        return ans.toString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
