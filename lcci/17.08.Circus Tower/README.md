---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/17.08.Circus%20Tower/README_EN.md
---

<!-- problem:start -->

# [17.08. Circus Tower](https://leetcode.cn/problems/circus-tower-lcci)

[Chinese Version](/lcci/17.08.Circus%20Tower/README.md)

## Description

<!-- description:start -->

<p>A circus is designing a tower routine consisting of people standing atop one anoth&shy;er&#39;s shoulders. For practical and aesthetic reasons, each person must be both shorter and lighter than the person below him or her. Given the heights and weights of each person in the circus, write a method to compute the largest possible number of people in such a tower.</p>
<p><strong>Example: </strong></p>
<pre>

<strong>Input: </strong>height = [65,70,56,75,60,68] weight = [100,150,90,190,95,110]

<strong>Output: </strong>6

<strong>Explanation: </strong>The longest tower is length 6 and includes from top to bottom: (56,90), (60,95), (65,100), (68,110), (70,150), (75,190)</pre>

<p>Note:</p>
<ul>
	<li><code>height.length == weight.length &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + Discretization + Binary Indexed Tree

First, we sort all people in ascending order by height. If the heights are the same, we sort them in descending order by weight. This way, we can transform the problem into finding the longest increasing subsequence of the weight array.

The longest increasing subsequence problem can be solved using dynamic programming with a time complexity of $O(n^2)$. However, we can optimize the solution process using a Binary Indexed Tree, which reduces the time complexity to $O(n \log n)$.

The space complexity is $O(n)$, where $n$ is the number of people.

<!-- tabs:start -->

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int val) {
        while (x <= n) {
            this.c[x] = Math.max(this.c[x], val);
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s = Math.max(s, this.c[x]);
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int bestSeqAtIndex(int[] height, int[] weight) {
        int n = height.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {height[i], weight[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        Set<Integer> s = new HashSet<>();
        for (int[] e : arr) {
            s.add(e[1]);
        }
        List<Integer> alls = new ArrayList<>(s);
        Collections.sort(alls);
        Map<Integer, Integer> m = new HashMap<>(alls.size());
        for (int i = 0; i < alls.size(); ++i) {
            m.put(alls.get(i), i + 1);
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(alls.size());
        int ans = 1;
        for (int[] e : arr) {
            int x = m.get(e[1]);
            int t = tree.query(x - 1) + 1;
            ans = Math.max(ans, t);
            tree.update(x, t);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
