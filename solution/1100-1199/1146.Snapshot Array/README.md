---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1100-1199/1146.Snapshot%20Array/README_EN.md
rating: 1770
source: Weekly Contest 148 Q3
tags:
    - Design
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [1146. Snapshot Array](https://leetcode.com/problems/snapshot-array)

[Chinese Version](/solution/1100-1199/1146.Snapshot%20Array/README.md)

## Description

<!-- description:start -->

<p>Implement a SnapshotArray that supports the following interface:</p>

<ul>
	<li><code>SnapshotArray(int length)</code> initializes an array-like data structure with the given length. <strong>Initially, each element equals 0</strong>.</li>
	<li><code>void set(index, val)</code> sets the element at the given <code>index</code> to be equal to <code>val</code>.</li>
	<li><code>int snap()</code> takes a snapshot of the array and returns the <code>snap_id</code>: the total number of times we called <code>snap()</code> minus <code>1</code>.</li>
	<li><code>int get(index, snap_id)</code> returns the value at the given <code>index</code>, at the time we took the snapshot with the given <code>snap_id</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> [&quot;SnapshotArray&quot;,&quot;set&quot;,&quot;snap&quot;,&quot;set&quot;,&quot;get&quot;]
[[3],[0,5],[],[0,6],[0,0]]
<strong>Output:</strong> [null,null,0,null,5]
<strong>Explanation: </strong>
SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3
snapshotArr.set(0,5);  // Set array[0] = 5
snapshotArr.snap();  // Take a snapshot, return snap_id = 0
snapshotArr.set(0,6);
snapshotArr.get(0,0);  // Get the value of array[0] with snap_id = 0, return 5</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= index &lt; length</code></li>
	<li><code>0 &lt;= val &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= snap_id &lt; </code>(the total number of times we call <code>snap()</code>)</li>
	<li>At most <code>5 * 10<sup>4</sup></code> calls will be made to <code>set</code>, <code>snap</code>, and <code>get</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Array + Binary Search

We maintain an array of length `length`. Each element in the array is a list, which is used to store the value set each time and the corresponding snapshot ID.

When the `set` method is called, we add the value and snapshot ID to the list at the corresponding index. The time complexity is $O(1)$.

When the `snap` method is called, we first increment the snapshot ID, then return the snapshot ID minus one. The time complexity is $O(1)$.

When the `get` method is called, we use binary search to find the first snapshot ID greater than `snap_id` at the corresponding position, and then return the previous value. If it cannot be found, return 0. The time complexity is $O(\log n)$.

The space complexity is $O(n)$.

<!-- tabs:start -->

#### Java

```java
class SnapshotArray {
    private List<int[]>[] arr;
    private int idx;

    public SnapshotArray(int length) {
        arr = new List[length];
        Arrays.setAll(arr, k -> new ArrayList<>());
    }

    public void set(int index, int val) {
        arr[index].add(new int[] {idx, val});
    }

    public int snap() {
        return idx++;
    }

    public int get(int index, int snap_id) {
        int l = 0, r = arr[index].size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (arr[index].get(mid)[0] > snap_id) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        --l;
        return l < 0 ? 0 : arr[index].get(l)[1];
    }
}

/**
 * Your SnapshotArray object will be instantiated and called as such:
 * SnapshotArray obj = new SnapshotArray(length);
 * obj.set(index,val);
 * int param_2 = obj.snap();
 * int param_3 = obj.get(index,snap_id);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
