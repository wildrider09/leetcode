---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/03.05.Sort%20of%20Stacks/README_EN.md
---

<!-- problem:start -->

# [03.05. Sort of Stacks](https://leetcode.cn/problems/sort-of-stacks-lcci)

[Chinese Version](/lcci/03.05.Sort%20of%20Stacks/README.md)

## Description

<!-- description:start -->

<p>Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: <code>push</code>, <code>pop</code>, <code>peek</code>, and <code>isEmpty</code>. When the stack is empty, <code>peek</code> should return -1.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>:

[&quot;SortedStack&quot;, &quot;push&quot;, &quot;push&quot;, &quot;peek&quot;, &quot;pop&quot;, &quot;peek&quot;]

[[], [1], [2], [], [], []]

<strong> Output</strong>:

[null,null,null,1,null,2]

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>:

[&quot;SortedStack&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;isEmpty&quot;]

[[], [], [], [1], [], []]

<strong> Output</strong>:

[null,null,null,null,null,true]

</pre>

<p><strong>Note:</strong></p>

<ol>
	<li>The total number of elements in the stack is within the range [0, 5000].</li>
</ol>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Stack + Auxiliary Stack

We define a stack $stk$ for storing elements.

In the `push` operation, we define an auxiliary stack $t$ for storing elements in $stk$ that are smaller than the current element. We pop all elements smaller than the current element from $stk$ and store them in $t$, then push the current element into $stk$, and finally pop all elements from $t$ and push them back into $stk$. The time complexity is $O(n)$.

In the `pop` operation, we just need to check if $stk$ is empty. If it's not, we pop the top element. The time complexity is $O(1)$.

In the `peek` operation, we just need to check if $stk$ is empty. If it is, we return -1, otherwise, we return the top element. The time complexity is $O(1)$.

In the `isEmpty` operation, we just need to check if $stk$ is empty. The time complexity is $O(1)$.

The space complexity is $O(n)$, where $n$ is the number of elements in the stack.

<!-- tabs:start -->

#### Java

```java
class SortedStack {
    private Deque<Integer> stk = new ArrayDeque<>();

    public SortedStack() {
    }

    public void push(int val) {
        Deque<Integer> t = new ArrayDeque<>();
        while (!stk.isEmpty() && stk.peek() < val) {
            t.push(stk.pop());
        }
        stk.push(val);
        while (!t.isEmpty()) {
            stk.push(t.pop());
        }
    }

    public void pop() {
        if (!isEmpty()) {
            stk.pop();
        }
    }

    public int peek() {
        return isEmpty() ? -1 : stk.peek();
    }

    public boolean isEmpty() {
        return stk.isEmpty();
    }
}

/**
 * Your SortedStack object will be instantiated and called as such:
 * SortedStack obj = new SortedStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.isEmpty();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
