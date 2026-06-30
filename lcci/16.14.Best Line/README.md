---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/16.14.Best%20Line/README_EN.md
---

<!-- problem:start -->

# [16.14. Best Line](https://leetcode.cn/problems/best-line-lcci)

[Chinese Version](/lcci/16.14.Best%20Line/README.md)

## Description

<!-- description:start -->

<p>Given a two-dimensional graph with points on it, find a line which passes the most number of points.</p>
<p>Assume all the points that passed by the line are stored in list <code>S</code>&nbsp;sorted by their number. You need to return <code>[S[0], S[1]]</code>, that is , two points that have smallest number. If there are more than one line that passes the most number of points, choose the one that has the smallest <code>S[0].</code>&nbsp;If there are more that one line that has the same <code>S[0]</code>, choose the one that has smallest <code>S[1]</code>.</p>
<p><strong>Example: </strong></p>
<pre>

<strong>Input: </strong> [[0,0],[1,1],[1,0],[2,0]]

<strong>Output: </strong> [0,2]

<strong>Explanation: </strong> The numbers of points passed by the line are [0,2,3].

</pre>
<p><strong>Note: </strong></p>
<ul>
	<li><code>2 &lt;= len(Points) &lt;= 300</code></li>
	<li><code>len(Points[i]) = 2</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Brute Force

We can enumerate any two points $(x_1, y_1), (x_2, y_2)$, connect these two points into a line, and the number of points on this line is 2. Then we enumerate other points $(x_3, y_3)$, and determine whether they are on the same line. If they are, the number of points on the line increases by 1; otherwise, the number of points on the line remains the same. Find the maximum number of points on a line, and the corresponding smallest two point indices are the answer.

The time complexity is $O(n^3)$, and the space complexity is $O(1)$. Here, $n$ is the length of the array `points`.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] bestLine(int[][] points) {
        int n = points.length;
        int mx = 0;
        int[] ans = new int[2];
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int cnt = 2;
                for (int k = j + 1; k < n; ++k) {
                    int x3 = points[k][0], y3 = points[k][1];
                    int a = (y2 - y1) * (x3 - x1);
                    int b = (y3 - y1) * (x2 - x1);
                    if (a == b) {
                        ++cnt;
                    }
                }
                if (mx < cnt) {
                    mx = cnt;
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Enumeration + Hash Table

We can enumerate a point $(x_1, y_1)$, store the slope of the line connecting $(x_1, y_1)$ and all other points $(x_2, y_2)$ in a hash table. Points with the same slope are on the same line, and the key of the hash table is the slope, and the value is the number of points on the line. Find the maximum value in the hash table, which is the answer. To avoid precision issues, we can reduce the slope $\frac{y_2 - y_1}{x_2 - x_1}$, and the reduction method is to find the greatest common divisor, and then divide the numerator and denominator by the greatest common divisor. The resulting numerator and denominator are used as the key of the hash table.

The time complexity is $O(n^2 \times \log m)$, and the space complexity is $O(n)$. Here, $n$ and $m$ are the length of the array `points` and the maximum difference between all horizontal and vertical coordinates in the array `points`, respectively.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] bestLine(int[][] points) {
        int n = points.length;
        int mx = 0;
        int[] ans = new int[2];
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            Map<String, List<int[]>> cnt = new HashMap<>();
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int dx = x2 - x1, dy = y2 - y1;
                int g = gcd(dx, dy);
                String key = (dx / g) + "." + (dy / g);
                cnt.computeIfAbsent(key, k -> new ArrayList<>()).add(new int[] {i, j});
                if (mx < cnt.get(key).size()
                    || (mx == cnt.get(key).size()
                        && (ans[0] > cnt.get(key).get(0)[0]
                            || (ans[0] == cnt.get(key).get(0)[0]
                                && ans[1] > cnt.get(key).get(0)[1])))) {
                    mx = cnt.get(key).size();
                    ans = cnt.get(key).get(0);
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
