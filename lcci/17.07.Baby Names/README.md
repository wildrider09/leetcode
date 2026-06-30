---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.07.Baby%20Names/README_EN.md
---

<!-- problem:start -->

# [17.07. Baby Names](https://leetcode.cn/problems/baby-names-lcci)

[Chinese Version](/lcci/17.07.Baby%20Names/README.md)

## Description

<!-- description:start -->

<p>Each year, the government releases a list of the 10000 most common baby names and their frequencies (the number of babies with that name). The only problem with this is that some names have multiple spellings. For example,&quot;John&quot; and &#39;&#39;Jon&quot; are essentially the same name but would be listed separately in the list. Given two lists, one of names/frequencies and the other of pairs of equivalent names, write an algorithm to print a new list of the true frequency of each name. Note that if John and Jon are synonyms, and Jon and Johnny are synonyms, then John and Johnny are synonyms. (It is both transitive and symmetric.) In the final list, choose the name that are <strong>lexicographically smallest</strong> as the &quot;real&quot; name.</p>

<p><strong>Example: </strong></p>

<pre>

<strong>Input: </strong>names = [&quot;John(15)&quot;,&quot;Jon(12)&quot;,&quot;Chris(13)&quot;,&quot;Kris(4)&quot;,&quot;Christopher(19)&quot;], synonyms = [&quot;(Jon,John)&quot;,&quot;(John,Johnny)&quot;,&quot;(Chris,Kris)&quot;,&quot;(Chris,Christopher)&quot;]

<strong>Output: </strong>[&quot;John(27)&quot;,&quot;Chris(36)&quot;]</pre>

<p>Note:</p>

<ul>
	<li><code>names.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + DFS

For each pair of synonyms, we establish bidirectional edges between the two names and store them in the adjacency list $g$. Then, we traverse all names, store them in the set $s$, and store their frequencies in the hash table $cnt$.

Next, we traverse each name in the set $s$. If the name has not been visited, we perform a depth-first search to find all names in the connected component where the name is located. We use the name with the smallest lexicographic order as the real name, and the sum of their frequencies is the frequency of the real name. Then, we store this name and its frequency in the answer array.

After traversing all names, the answer array is what we seek.

The time complexity is $O(n + m)$, and the space complexity is $O(n + m)$. Where $n$ and $m$ are the lengths of the name array and the synonym array, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    private Map<String, List<String>> g = new HashMap<>();
    private Map<String, Integer> cnt = new HashMap<>();
    private Set<String> vis = new HashSet<>();
    private int freq;

    public String[] trulyMostPopular(String[] names, String[] synonyms) {
        for (String pairs : synonyms) {
            String[] pair = pairs.substring(1, pairs.length() - 1).split(",");
            String a = pair[0], b = pair[1];
            g.computeIfAbsent(a, k -> new ArrayList<>()).add(b);
            g.computeIfAbsent(b, k -> new ArrayList<>()).add(a);
        }
        Set<String> s = new HashSet<>();
        for (String x : names) {
            int i = x.indexOf('(');
            String name = x.substring(0, i);
            s.add(name);
            cnt.put(name, Integer.parseInt(x.substring(i + 1, x.length() - 1)));
        }
        List<String> res = new ArrayList<>();
        for (String name : s) {
            if (!vis.contains(name)) {
                freq = 0;
                name = dfs(name);
                res.add(name + "(" + freq + ")");
            }
        }
        return res.toArray(new String[0]);
    }

    private String dfs(String a) {
        String mi = a;
        vis.add(a);
        freq += cnt.getOrDefault(a, 0);
        for (String b : g.getOrDefault(a, new ArrayList<>())) {
            if (!vis.contains(b)) {
                String t = dfs(b);
                if (t.compareTo(mi) < 0) {
                    mi = t;
                }
            }
        }
        return mi;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
