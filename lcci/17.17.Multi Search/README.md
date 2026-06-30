---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.17.Multi%20Search/README_EN.md
---

<!-- problem:start -->

# [17.17. Multi Search](https://leetcode.cn/problems/multi-search-lcci)

[Chinese Version](/lcci/17.17.Multi%20Search/README.md)

## Description

<!-- description:start -->

<p>Given a string band an array of smaller strings T, design a method to search b for each small string in T. Output&nbsp;<code>positions</code> of all strings in&nbsp;<code>smalls</code>&nbsp;that appear in <code>big</code>,&nbsp;where <code>positions[i]</code> is all positions of <code>smalls[i]</code>.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>

big = &quot;mississippi&quot;

smalls = [&quot;is&quot;,&quot;ppi&quot;,&quot;hi&quot;,&quot;sis&quot;,&quot;i&quot;,&quot;ssippi&quot;]

<strong>Output: </strong> [[1,4],[8],[],[3],[1,4,7,10],[5]]

</pre>

<p><strong>Note: </strong></p>

<ul>
	<li><code>0 &lt;= len(big) &lt;= 1000</code></li>
	<li><code>0 &lt;= len(smalls[i]) &lt;= 1000</code></li>
	<li>The total number of characters in&nbsp;<code>smalls</code>&nbsp;will not exceed 100000.</li>
	<li>No duplicated strings in&nbsp;<code>smalls</code>.</li>
	<li>All characters are lowercase letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[][] multiSearch(String big, String[] smalls) {
        Trie tree = new Trie();
        int n = smalls.length;
        for (int i = 0; i < n; ++i) {
            tree.insert(smalls[i], i);
        }
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            res.add(new ArrayList<>());
        }
        for (int i = 0; i < big.length(); ++i) {
            String s = big.substring(i);
            List<Integer> t = tree.search(s);
            for (int idx : t) {
                res.get(idx).add(i);
            }
        }
        int[][] ans = new int[n][];
        for (int i = 0; i < n; ++i) {
            ans[i] = res.get(i).stream().mapToInt(Integer::intValue).toArray();
        }
        return ans;
    }
}

class Trie {
    private int idx;
    private Trie[] children;

    public Trie() {
        idx = -1;
        children = new Trie[26];
    }

    public void insert(String word, int i) {
        Trie node = this;
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                node.children[c] = new Trie();
            }
            node = node.children[c];
        }
        node.idx = i;
    }

    public List<Integer> search(String word) {
        Trie node = this;
        List<Integer> res = new ArrayList<>();
        for (char c : word.toCharArray()) {
            c -= 'a';
            if (node.children[c] == null) {
                return res;
            }
            node = node.children[c];
            if (node.idx != -1) {
                res.add(node.idx);
            }
        }
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
