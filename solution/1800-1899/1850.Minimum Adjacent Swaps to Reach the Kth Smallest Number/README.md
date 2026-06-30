---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/1800-1899/1850.Minimum%20Adjacent%20Swaps%20to%20Reach%20the%20Kth%20Smallest%20Number/README_EN.md
rating: 2073
source: Weekly Contest 239 Q3
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1850. Minimum Adjacent Swaps to Reach the Kth Smallest Number](https://leetcode.com/problems/minimum-adjacent-swaps-to-reach-the-kth-smallest-number)

[Chinese Version](/solution/1800-1899/1850.Minimum%20Adjacent%20Swaps%20to%20Reach%20the%20Kth%20Smallest%20Number/README.md)

## Description

<!-- description:start -->

<p>You are given a string <code>num</code>, representing a large integer, and an integer <code>k</code>.</p>

<p>We call some integer <strong>wonderful</strong> if it is a <strong>permutation</strong> of the digits in <code>num</code> and is <strong>greater in value</strong> than <code>num</code>. There can be many wonderful integers. However, we only care about the <strong>smallest-valued</strong> ones.</p>

<ul>
	<li>For example, when <code>num = &quot;5489355142&quot;</code>:

    <ul>
    	<li>The 1<sup>st</sup> smallest wonderful integer is <code>&quot;5489355214&quot;</code>.</li>
    	<li>The 2<sup>nd</sup> smallest wonderful integer is <code>&quot;5489355241&quot;</code>.</li>
    	<li>The 3<sup>rd</sup> smallest wonderful integer is <code>&quot;5489355412&quot;</code>.</li>
    	<li>The 4<sup>th</sup> smallest wonderful integer is <code>&quot;5489355421&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>Return <em>the <strong>minimum number of adjacent digit swaps</strong> that needs to be applied to </em><code>num</code><em> to reach the </em><code>k<sup>th</sup></code><em><strong> smallest wonderful</strong> integer</em>.</p>

<p>The tests are generated in such a way that <code>k<sup>th</sup></code>&nbsp;smallest wonderful integer exists.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;5489355142&quot;, k = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong> The 4<sup>th</sup> smallest wonderful number is &quot;5489355421&quot;. To get this number:
- Swap index 7 with index 8: &quot;5489355<u>14</u>2&quot; -&gt; &quot;5489355<u>41</u>2&quot;
- Swap index 8 with index 9: &quot;54893554<u>12</u>&quot; -&gt; &quot;54893554<u>21</u>&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;11112&quot;, k = 4
<strong>Output:</strong> 4
<strong>Explanation:</strong> The 4<sup>th</sup> smallest wonderful number is &quot;21111&quot;. To get this number:
- Swap index 3 with index 4: &quot;111<u>12</u>&quot; -&gt; &quot;111<u>21</u>&quot;
- Swap index 2 with index 3: &quot;11<u>12</u>1&quot; -&gt; &quot;11<u>21</u>1&quot;
- Swap index 1 with index 2: &quot;1<u>12</u>11&quot; -&gt; &quot;1<u>21</u>11&quot;
- Swap index 0 with index 1: &quot;<u>12</u>111&quot; -&gt; &quot;<u>21</u>111&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> num = &quot;00123&quot;, k = 1
<strong>Output:</strong> 1
<strong>Explanation:</strong> The 1<sup>st</sup> smallest wonderful number is &quot;00132&quot;. To get this number:
- Swap index 3 with index 4: &quot;001<u>23</u>&quot; -&gt; &quot;001<u>32</u>&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= num.length &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>num</code> only consists of digits.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Find Next Permutation + Inversion Pairs

We can call the `next_permutation` function $k$ times to get the $k$th smallest permutation $s$.

Next, we just need to calculate how many swaps are needed for $num$ to become $s$.

Let's first consider a simple situation where all the digits in $num$ are different. In this case, we can directly map the digit characters in $num$ to indices. For example, if $num$ is `"54893"` and $s$ is `"98345"`. We map each digit in $num$ to an index, that is:

$$
\begin{aligned}
num[0] &= 5 &\rightarrow& \quad 0 \\
num[1] &= 4 &\rightarrow& \quad 1 \\
num[2] &= 8 &\rightarrow& \quad 2 \\
num[3] &= 9 &\rightarrow& \quad 3 \\
num[4] &= 3 &\rightarrow& \quad 4 \\
\end{aligned}
$$

Then, mapping each digit in $s$ to an index results in `"32410"`. In this way, the number of swaps needed to change $num$ to $s$ is equal to the number of inversion pairs in the index array after $s$ is mapped.

If there are identical digits in $num$, we can use an array $d$ to record the indices where each digit appears, where $d[i]$ represents the list of indices where the digit $i$ appears. To minimize the number of swaps, when mapping $s$ to an index array, we only need to greedily select the index of the corresponding digit in $d$ in order.

Finally, we can directly use a double loop to calculate the number of inversion pairs, or we can optimize it with a Binary Indexed Tree.

The time complexity is $O(n \times (k + n))$, and the space complexity is $O(n)$. Where $n$ is the length of $num$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int getMinSwaps(String num, int k) {
        char[] s = num.toCharArray();
        for (int i = 0; i < k; ++i) {
            nextPermutation(s);
        }
        List<Integer>[] d = new List[10];
        Arrays.setAll(d, i -> new ArrayList<>());
        int n = s.length;
        for (int i = 0; i < n; ++i) {
            d[num.charAt(i) - '0'].add(i);
        }
        int[] idx = new int[10];
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = d[s[i] - '0'].get(idx[s[i] - '0']++);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (arr[j] > arr[i]) {
                    ++ans;
                }
            }
        }
        return ans;
    }

    private boolean nextPermutation(char[] nums) {
        int n = nums.length;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            --i;
        }
        if (i < 0) {
            return false;
        }
        int j = n - 1;
        while (j >= 0 && nums[i] >= nums[j]) {
            --j;
        }
        swap(nums, i++, j);
        for (j = n - 1; i < j; ++i, --j) {
            swap(nums, i, j);
        }
        return true;
    }

    private void swap(char[] nums, int i, int j) {
        char t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
