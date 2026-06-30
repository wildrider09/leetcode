---
comments: true
difficulty: Hard
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0700-0799/0759.Employee%20Free%20Time/README_EN.md
tags:
    - Array
    - Sorting
    - Sweep Line
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [759. Employee Free Time 🔒](https://leetcode.com/problems/employee-free-time)

[Chinese Version](/solution/0700-0799/0759.Employee%20Free%20Time/README.md)

## Description

<!-- description:start -->

<p>We are given a list <code>schedule</code> of employees, which represents the working time for each employee.</p>

<p>Each employee has a list of non-overlapping <code>Intervals</code>, and these intervals are in sorted order.</p>

<p>Return the list of finite intervals representing <b>common, positive-length free time</b> for <i>all</i> employees, also in sorted order.</p>

<p>(Even though we are representing <code>Intervals</code> in the form <code>[x, y]</code>, the objects inside are <code>Intervals</code>, not lists or arrays. For example, <code>schedule[0][0].start = 1</code>, <code>schedule[0][0].end = 2</code>, and <code>schedule[0][0][0]</code> is not defined).&nbsp; Also, we wouldn&#39;t include intervals like [5, 5] in our answer, as they have zero length.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> schedule = [[[1,2],[5,6]],[[1,3]],[[4,10]]]
<strong>Output:</strong> [[3,4]]
<strong>Explanation:</strong> There are a total of three employees, and all common
free time intervals would be [-inf, 1], [3, 4], [10, inf].
We discard any intervals that contain inf as they aren&#39;t finite.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> schedule = [[[1,3],[6,7]],[[2,4]],[[2,5],[9,12]]]
<strong>Output:</strong> [[5,6],[7,9]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= schedule.length , schedule[i].length &lt;= 50</code></li>
	<li><code>0 &lt;= schedule[i].start &lt; schedule[i].end &lt;= 10^8</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Interval Merging

We can merge all employees' working time intervals into a single list, then sort and merge the overlapping intervals. Finally, we traverse the merged interval list to find the free time periods between adjacent intervals.

The time complexity is $O(mn \log(mn))$ and the space complexity is $O(mn)$, where $m$ is the number of employees and $n$ is the number of working intervals per employee.

<!-- tabs:start -->

#### Java

```java
/*
// Definition for an Interval.
class Interval {
    public int start;
    public int end;

    public Interval() {}

    public Interval(int _start, int _end) {
        start = _start;
        end = _end;
    }
};
*/

class Solution {
    public List<Interval> employeeFreeTime(List<List<Interval>> schedule) {
        List<Interval> intervals = new ArrayList<>();
        for (List<Interval> e : schedule) {
            intervals.addAll(e);
        }

        intervals.sort((a, b) -> a.start == b.start ? a.end - b.end : a.start - b.start);

        List<Interval> merged = new ArrayList<>();
        merged.add(intervals.get(0));
        for (int i = 1; i < intervals.size(); ++i) {
            Interval last = merged.get(merged.size() - 1);
            Interval cur = intervals.get(i);
            if (last.end < cur.start) {
                merged.add(cur);
            } else {
                last.end = Math.max(last.end, cur.end);
            }
        }

        List<Interval> ans = new ArrayList<>();
        for (int i = 1; i < merged.size(); ++i) {
            Interval a = merged.get(i - 1);
            Interval b = merged.get(i);
            ans.add(new Interval(a.end, b.start));
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
