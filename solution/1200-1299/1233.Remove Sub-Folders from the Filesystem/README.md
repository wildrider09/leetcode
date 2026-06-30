---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1200-1299/1233.Remove%20Sub-Folders%20from%20the%20Filesystem/README_EN.md
rating: 1544
source: Weekly Contest 159 Q2
tags:
    - Depth-First Search
    - Trie
    - Array
    - String
---

<!-- problem:start -->

# [1233. Remove Sub-Folders from the Filesystem](https://leetcode.com/problems/remove-sub-folders-from-the-filesystem)

[Chinese Version](/solution/1200-1299/1233.Remove%20Sub-Folders%20from%20the%20Filesystem/README.md)

## Description

<!-- description:start -->

<p>Given a list of folders <code>folder</code>, return <em>the folders after removing all <strong>sub-folders</strong> in those folders</em>. You may return the answer in <strong>any order</strong>.</p>

<p>If a <code>folder[i]</code> is located within another <code>folder[j]</code>, it is called a <strong>sub-folder</strong> of it. A sub-folder of <code>folder[j]</code> must start with <code>folder[j]</code>, followed by a <code>&quot;/&quot;</code>. For example, <code>&quot;/a/b&quot;</code> is a sub-folder of <code>&quot;/a&quot;</code>, but <code>&quot;/b&quot;</code> is not a sub-folder of <code>&quot;/a/b/c&quot;</code>.</p>

<p>The format of a path is one or more concatenated strings of the form: <code>&#39;/&#39;</code> followed by one or more lowercase English letters.</p>

<ul>
	<li>For example, <code>&quot;/leetcode&quot;</code> and <code>&quot;/leetcode/problems&quot;</code> are valid paths while an empty string and <code>&quot;/&quot;</code> are not.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> folder = [&quot;/a&quot;,&quot;/a/b&quot;,&quot;/c/d&quot;,&quot;/c/d/e&quot;,&quot;/c/f&quot;]
<strong>Output:</strong> [&quot;/a&quot;,&quot;/c/d&quot;,&quot;/c/f&quot;]
<strong>Explanation:</strong> Folders &quot;/a/b&quot; is a subfolder of &quot;/a&quot; and &quot;/c/d/e&quot; is inside of folder &quot;/c/d&quot; in our filesystem.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> folder = [&quot;/a&quot;,&quot;/a/b/c&quot;,&quot;/a/b/d&quot;]
<strong>Output:</strong> [&quot;/a&quot;]
<strong>Explanation:</strong> Folders &quot;/a/b/c&quot; and &quot;/a/b/d&quot; will be removed because they are subfolders of &quot;/a&quot;.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> folder = [&quot;/a/b/c&quot;,&quot;/a/b/ca&quot;,&quot;/a/b/d&quot;]
<strong>Output:</strong> [&quot;/a/b/c&quot;,&quot;/a/b/ca&quot;,&quot;/a/b/d&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= folder.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>2 &lt;= folder[i].length &lt;= 100</code></li>
	<li><code>folder[i]</code> contains only lowercase letters and <code>&#39;/&#39;</code>.</li>
	<li><code>folder[i]</code> always starts with the character <code>&#39;/&#39;</code>.</li>
	<li>Each folder name is <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting

First, we sort the array `folder` in lexicographical order, then traverse the array. For the current folder $f$ we are traversing, if its length is greater than or equal to the length of the last folder in the answer array, and its prefix includes the last folder in the answer array plus a `/`, then $f$ is a subfolder of the last folder in the answer array, and we don't need to add it to the answer array. Otherwise, we add $f$ to the answer array.

After the traversal ends, the folders in the answer array are the answer required by the problem.

The time complexity is $O(n \times \log n \times m)$, and the space complexity is $O(m)$. Where $n$ and $m$ are the length of the array `folder` and the maximum length of the strings in the array `folder`, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Arrays.sort(folder);
        List<String> ans = new ArrayList<>();
        ans.add(folder[0]);
        for (int i = 1; i < folder.length; ++i) {
            int m = ans.get(ans.size() - 1).length();
            int n = folder[i].length();
            if (m >= n
                || !(ans.get(ans.size() - 1).equals(folder[i].substring(0, m))
                    && folder[i].charAt(m) == '/')) {
                ans.add(folder[i]);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Trie

We can use a trie to store all the folders in the array `folder`. Each node of the trie contains a `children` field, used to store the child nodes of the current node, and a `fid` field, used to store the index of the folder corresponding to the current node in the array `folder`.

For each folder $f$ in the array `folder`, we first split $f$ into several substrings according to `/`, then start from the root node and add the substrings to the trie in turn. Next, we start from the root node to search the trie. If the `fid` field of the current node is not `-1`, it means that the folder corresponding to the current node is a folder in the answer array. We add it to the answer array and return. Otherwise, we recursively search all child nodes of the current node and finally return the answer array.

The time complexity is $O(n \times m)$, and the space complexity is $O(n \times m)$. Where $n$ and $m$ are the length of the array `folder` and the maximum length of the strings in the array `folder`, respectively.

<!-- tabs:start -->

#### Java

```java
class Trie {
    private Map<String, Trie> children = new HashMap<>();
    private int fid = -1;

    public void insert(int fid, String f) {
        Trie node = this;
        String[] ps = f.split("/");
        for (int i = 1; i < ps.length; ++i) {
            String p = ps[i];
            if (!node.children.containsKey(p)) {
                node.children.put(p, new Trie());
            }
            node = node.children.get(p);
        }
        node.fid = fid;
    }

    public List<Integer> search() {
        List<Integer> ans = new ArrayList<>();
        dfs(this, ans);
        return ans;
    }

    private void dfs(Trie root, List<Integer> ans) {
        if (root.fid != -1) {
            ans.add(root.fid);
            return;
        }
        for (var child : root.children.values()) {
            dfs(child, ans);
        }
    }
}

class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Trie trie = new Trie();
        for (int i = 0; i < folder.length; ++i) {
            trie.insert(i, folder[i]);
        }
        List<String> ans = new ArrayList<>();
        for (int i : trie.search()) {
            ans.add(folder[i]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
