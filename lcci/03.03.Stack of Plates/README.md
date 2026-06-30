---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/03.03.Stack%20of%20Plates/README_EN.md
---

<!-- problem:start -->

# [03.03. Stack of Plates](https://leetcode.cn/problems/stack-of-plates-lcci)

[Chinese Version](/lcci/03.03.Stack%20of%20Plates/README.md)

## Description

<!-- description:start -->

<p>Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure <code>SetOfStacks</code> that mimics this.&nbsp;<code>SetOfStacks</code> should be composed of several stacks and should create a new stack once the previous one exceeds capacity. <code>SetOfStacks.push()</code> and <code>SetOfStacks.pop()</code> should behave identically to a single stack (that is, <code>pop()</code> should return the same values as it would if there were just a single stack). Follow Up: Implement a function <code>popAt(int index)</code> which performs a pop operation on a specific sub-stack.</p>
<p>You should delete the sub-stack when it becomes empty. <code>pop</code>, <code>popAt</code> should return -1 when there&#39;s no element to pop.</p>
<p><strong>Example1:</strong></p>
<pre>

<strong> Input</strong>:

[&quot;StackOfPlates&quot;, &quot;push&quot;, &quot;push&quot;, &quot;popAt&quot;, &quot;pop&quot;, &quot;pop&quot;]

[[1], [1], [2], [1], [], []]

<strong> Output</strong>:

[null, null, null, 2, 1, -1]

<strong> Explanation</strong>:

</pre>
<p><strong>Example2:</strong></p>
<pre>

<strong> Input</strong>:

[&quot;StackOfPlates&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;popAt&quot;, &quot;popAt&quot;, &quot;popAt&quot;]

[[2], [1], [2], [3], [0], [0], [0]]

<strong> Output</strong>:

[null, null, null, null, 2, 1, 3]

</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

We can use a list of stacks $stk$ to simulate this process, initially $stk$ is empty.

- When the `push` method is called, if $cap$ is 0, return directly. Otherwise, if $stk$ is empty or the length of the last stack in $stk$ is greater than or equal to $cap$, then create a new stack. Then add the element $val$ to the last stack in $stk$. The time complexity is $O(1)$.
- When the `pop` method is called, return the result of `popAt(|stk| - 1)`. The time complexity is $O(1)$.
- When the `popAt` method is called, if $index$ is not in the range $[0, |stk|)$, return -1. Otherwise, return the top element of $stk[index]$ and pop it out. If $stk[index]$ is empty after popping, remove it from $stk$. The time complexity is $O(1)$.

The space complexity is $O(n)$, where $n$ is the number of elements.

<!-- tabs:start -->

#### Java

```java
class StackOfPlates {
    private List<Deque<Integer>> stk = new ArrayList<>();
    private int cap;

    public StackOfPlates(int cap) {
        this.cap = cap;
    }

    public void push(int val) {
        if (cap == 0) {
            return;
        }
        if (stk.isEmpty() || stk.get(stk.size() - 1).size() >= cap) {
            stk.add(new ArrayDeque<>());
        }
        stk.get(stk.size() - 1).push(val);
    }

    public int pop() {
        return popAt(stk.size() - 1);
    }

    public int popAt(int index) {
        int ans = -1;
        if (index >= 0 && index < stk.size()) {
            ans = stk.get(index).pop();
            if (stk.get(index).isEmpty()) {
                stk.remove(index);
            }
        }
        return ans;
    }
}

/**
 * Your StackOfPlates object will be instantiated and called as such:
 * StackOfPlates obj = new StackOfPlates(cap);
 * obj.push(val);
 * int param_2 = obj.pop();
 * int param_3 = obj.popAt(index);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
